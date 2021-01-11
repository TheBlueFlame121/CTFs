# Uncollidable
## The challenge
This was a pretty straightforward challenge, we had a hmac based secure server that can store data. We can choose our own keys and data.<br>
```python=
import json
import os
from hashlib import sha256
from typing import Callable, Dict

H: Callable[[bytes], bytes] = lambda s: sha256(s).digest()


def get_mac(key: bytes, data: bytes) -> bytes:
    """Just a MAC generation function similar to HMAC.
    Reference link: https://en.wikipedia.org/wiki/HMAC#Implementation
    """
    if len(key) >= 32:
        key = H(key)

    if len(key) < 32:
        key += b"\x00" * (32 - len(key))

    inner_pad = bytearray(32)
    outer_pad = bytearray(32)
    for i in range(32):
        inner_pad[i] = key[i] ^ 0x20
        outer_pad[i] = key[i] ^ 0x21

    return H(outer_pad + H(inner_pad + data))


class SecureStorage:
    """Changes on data stored in this storage can be detected :) """

    def __init__(self, debug=False):
        self.keys: Dict[str, bytes] = {}  # key ID -> key
        self.data: Dict[bytes, bytes] = {}  # MAC -> data
        self.debug = debug
        if debug:
            # when debug mode is on, unique arguments to `store_data` will be
            # logged.
            self.store_data_args = set()

    def generate_key(self, key_id: str) -> None:
        """Generate a new key."""
        if key_id in self.keys:
            raise Exception("duplicated key id")
        self.keys[key_id] = os.urandom(32)

    def import_key(self, key_id: str, key: bytes) -> None:
        """Import an external key."""
        if key_id in self.keys:
            raise Exception("duplicated key id")
        self.keys[key_id] = key

    def store_data(self, key_id: str, data: bytes) -> bytes:
        """Store data and return a MAC that can be used to retrieve the data
        later."""
        if key_id not in self.keys:
            raise Exception("key not found")

        key = self.keys[key_id]
        if self.debug:
            self.store_data_args.add((key, data))

        mac = get_mac(key, data)
        if mac in self.data:
            raise Exception("data already stored")

        self.data[mac] = data
        return mac

    def retrieve_data(self, key_id: str, mac: bytes) -> bytes:
        """Retrieve data previously stored with `store_data`."""
        if key_id not in self.keys:
            raise Exception("key not found")
        if mac not in self.data:
            raise Exception("data not found")

        key = self.keys[key_id]
        data = self.data[mac]
        if get_mac(key, data) != mac:
            raise Exception("data has been tampered")
        return data

    def report_bug(self) -> int:
        """Claim your bounty here :) """
        if self.debug:
            # check for a massive collision bug
            if len(self.store_data_args) >= 10 and len(self.data) == 1:
                # check if collisions happened through different key sizes
                key_lengths = [len(key) for (key, _) in self.store_data_args]
                if min(key_lengths) < 32 <= max(key_lengths):
                    # Congrats :)
                    from secret import flag
                    return int.from_bytes(flag, "big")
        return 0


def main():
    ss = SecureStorage(debug=True)
    for _ in range(100):
        try:
            request = json.loads(input())

            if request["action"] == "generate_key":
                key_id = request["key_id"]
                ss.generate_key(key_id)
                print(json.dumps({}))

            if request["action"] == "import_key":
                key_id = request["key_id"]
                key = bytes.fromhex(request["key"])
                ss.import_key(key_id, key)
                print(json.dumps({}))

            elif request["action"] == "store_data":
                key_id = request["key_id"]
                data = bytes.fromhex(request["data"])
                mac = ss.store_data(key_id, data)
                print(json.dumps({"mac": mac.hex()}))

            elif request["action"] == "retrieve_data":
                key_id = request["key_id"]
                mac = bytes.fromhex(request["mac"])
                data = ss.retrieve_data(key_id, mac)
                print(json.dumps({"data": data.hex()}))

            elif request["action"] == "report_bug":
                print(json.dumps({"bounty": ss.report_bug()}))

        except EOFError:
            break
        except Exception as err:
            print(json.dumps({"error": str(err)}))


if __name__ == '__main__':
    main()
```

We need to find a large enough hmac collision and it will give us the flag.<br>

## Idea and Solution
I had personally never dealt with hmac before so I thought it would be better to spend some time looking up the generic attacks. This lead me to a rabbit hole which I spent quite some time on. The main difference between usual implementations of hmac and this challenge is that we can freely control the keys here, whereas in practice that is not the case. This meant that most stuff I read wasn't really useful here<br>
<br>
I initially thought there must be some pair of unrelated (key, data) that leads to their hmac being same, but this theory was quickly discounted when I realised that I would need a sha256 collision and a sha256 complete pre-image which was obviously impossible.<br>
<br>
Then I took a closer look at the padding scheme of the key. When the key is atleast 32 bytes long, they hash it. However when it is lower than 32 bytes, they pad it till 32 with null bytes, that too without checking if it already has nullbytes!. So in essence, the following keys:<br>
```python=
key1 = os.usrandom(29) + b'\x00'
key2 = key1 + b'\x00'
```
with the same data, would generate the same hmac! and this method is extensible for a maximum of 31 collision which is way more than the 10 required.<br>
<br>
I had a sneaking suspicion that given the relatively low number of solves, this challenge could not be that simple. This was confirmed by the following line in the `report_bug` method:<br>
```python=
if min(key_lengths) < 32 <= max(key_lengths):
```
We needed one of the keys to be atleast 32 bytes long. This would mean that the key would be hashed. So now we needed to find a seed, whose sha256 hash ends with 9 null bytes. If we found that we could send the keys as:<br>
```
key1 = seed
key2 = sha256(seed).digest()[:-1]
key3 = sha256(seed).digest()[:-2]
.
.
key10 = sha256(seed).digest()[:-9]
```
So now the question is how do we get such a seed?
Well I tried bruteforcing it (spoiler: it doesn't work). It took my script 4.5 hours to give me a seed whose sha256 ends with just four zeros. I tried looking up if the seed I needed was already used elsewhere, but maybe I didn't phrase it properly because I couldn't find anything.<br>
<br>
After the CTF was over, it came to my notice that blockchain uses seeds such that their sha256 ends with atleast 8 null bytes always! So if I were to search the block hashes using a bitcoin api, I had a 1/256 chance of finding a seed that would help me solve the challenge.<br>
<br>
A script I picked up from [Joseph Surin's Blog](https://www.josephsurin.me/posts/2020-11-23-dragon-ctf-2020-bit-flip-writeups#bit-flip-2), after slight modification gave the desired result within seconds. Once you get the seed the challenge is trivial, import keys as shown above, generate macs on the same data with the keys, and claim your flag!<br>
<br>
This was a simple yet cool challenge, I learned about HMAC and this property of blockchain both for the first time during this.

###### tags: `CTF` `HMAC` `Blockchain` `TetCTF` `2021`
