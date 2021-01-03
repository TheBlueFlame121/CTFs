# Unevaluated
## The Challenge
I really liked this challenge, mainly because once you read the solution it seems really trivial and yet there were only 3 solves during the CTF. Here we have to calculate a Discrete Logarithm with Gaussian Integers over Integers mod N.

I could not solve this challenge during the CTF, I was on the right track but I don't think I would've got the flag. So this writeup was made after the CTF when I've spent time going through rkm's writeup, and a bit of talking with hellman and NDH.

Link to rkm's writeup: [https://rkm0959.tistory.com/192](https://https://rkm0959.tistory.com/192)

### Parameter Generation
Looking at the `generate_params` method inside the `ComplexDiffieHellman` class we can see how the parameters are being generated. 
```python=
def generate_params(prime_length):
    # Warning: this may take some time :)
    while True:
        p = getPrime(prime_length)
        if p % 4 == 3:
            if p % 3 == 2:
                q = (p - 1) // 2
                r = (p + 1) // 12
                if isPrime(q) and isPrime(r):
                    break
            else:
                q = (p - 1) // 6
                r = (p + 1) // 4
                if isPrime(q) and isPrime(r):
                    break
    n = p ** 2
    order = p * q * r
    while True:
        re = getRandomRange(1, n)
        im = getRandomRange(1, n)
        g = complex_pow(Complex(re, im), 24, n)
        if (
                complex_pow(g, order, n) == Complex(1, 0)
                and complex_pow(g, order // p, n) != Complex(1, 0)
                and complex_pow(g, order // q, n) != Complex(1, 0)
                and complex_pow(g, order // r, n) != Complex(1, 0)
        ):
            return g, order, n
```
The modulus $n$ is  simply the square of a prime $p$ and $p \pmod 4 \equiv 3$. We are also given the order, after dividing it by 24. To compensate for it, they raise the generator to the power of 24 before finally giving it back to us. (This will be important later).

### Encryption
```python=
def main():
    from os import urandom
    private_key = urandom(32)
    k = int.from_bytes(private_key, "big")

    cdh = ComplexDiffieHellman(debug=True)
    print("g =", cdh.g)
    print("order =", cdh.order)
    print("n =", cdh.n)
    print("public_key =", cdh.get_public_key(k))

    # Solve the discrete logarithm problem if you want the flag :)
    from secret import flag
    from Crypto.Cipher import AES
    if len(flag) % 16 != 0:
        flag += b"\x00" * (16 - len(flag) % 16)
    print("encrypted_flag = ",
          AES.new(private_key, AES.MODE_ECB).encrypt(flag))
```
Inside the main function, we can see that a 32 byte private key is randomly generated, using which our flag is encrypted by AES ECB. We are given the Discrete log parameters and a public key, which is just the generator raised to the power of our secret key. So like the comment says, we just have to solve the discrete log to get the private key and then we can have our flag.

## Solution Idea
First off, as rkm points out in his writeup, there is a flaw in the key generation. The order of the ring is ~384 bits long but the AES private key generated is only 256 bits long. This means that we might not have to calculate the discrete log completely but we can compute it over some part such that it becomes 256 bits long.

Rkm does this by splitting the discrete log mod p, then mod r and using CRT to combine the result, you can read about it in his writeup which also contains a lot more mathematical explanations. NDH later pointed out that you can just compute dlog over the norm map, and since it is mod $p^2$, it will be sufficiently long. This is the method I have described here.

So, there exists a homomorphism from the Gaussian integers in this ring to Reals over the ring of integers mod $n$. This is called the norm map and is defined as:
$$
Norm(a + ib) = a^2 + b^2
$$
It's doesn't take much to verify that this indeed is a homomorphism.
Now we can simply compute discrete log in $Zmod(n)$. We can write a simple function for that:
```python=
def n(c):
    return (c.re**2 + c.im**2)%n
```

Once we are done with the dlog, we might have to adjust the value by adding the order a few times to find the correct key.

Note: While using Sage, both `public_key.log(g)` and `discrete_log(public_key, g)` cause Sage to crash for some reason. Which sucks because I thought my approach was wrong because of this. You can get it to work by using the `znlog` in pari via Sage, which is what I've done in the final script.

Another thing to note: The order of a generator in $Zmod(n)$ would be $p(p-1)$. If we factorise the order we get $2*p*x$ where x is another prime. Now recall how our generator was raised to 24 during parameter generation, this means that it's order now will only be $p*x$ instead of $2*p*x$. This was something I would've definitely missed even if I got through the dlog step.

With all of this theory, we can get the flag using the final script

## Script
This takes a few minutes to run in sage
```python=
from collections import namedtuple
from Crypto.Util.number import *
from Crypto.Cipher import AES

# Copied functions from source
Complex = namedtuple("Complex", ["re", "im"])

def complex_mult(c1, c2, modulus):
    return Complex(
        (c1.re * c2.re - c1.im * c2.im) % modulus,  # real part
        (c1.re * c2.im + c1.im * c2.re) % modulus,  # image part
    )


def complex_pow(c, exp, modulus):
    result = Complex(1, 0)
    while exp > 0:
        if exp & 1:
            result = complex_mult(result, c, modulus)
        c = complex_mult(c, c, modulus)
        exp >>= 1
    return result

# data
g = Complex(re=20878314020629522511110696411629430299663617500650083274468525283663940214962,
            im=16739915489749335460111660035712237713219278122190661324570170645550234520364)
order = 364822540633315669941067187619936391080373745485429146147669403317263780363306505857156064209602926535333071909491
n = 42481052689091692859661163257336968116308378645346086679008747728668973847769
public_key = Complex(re=11048898386036746197306883207419421777457078734258168057000593553461884996107,
                     im=34230477038891719323025391618998268890391645779869016241994899690290519616973)
encrypted_flag = b'\'{\xda\xec\xe9\xa4\xc1b\x96\x9a\x8b\x92\x85\xb6&p\xe6W\x8axC)\xa7\x0f(N\xa1\x0b\x05\x19@<T>L9!\xb7\x9e3\xbc\x99\xf0\x8f\xb3\xacZ:\xb3\x1c\xb9\xb7;\xc7\x8a:\xb7\x10\xbd\x07"\xad\xc5\x84'

def f(c):
    return (c.re**2 + c.im**2)%n

print('[+] Computing Discrete Log')

# Passing it to Pari as sage will result in a crash
k = int(pari(f"znlog({f(public_key)}, Mod({f(g)}, {n}))"))

assert pow(f(g), k, n) == f(public_key)
print(f'[+] Discrete log result: {k}')

# computing the order
p = sqrt(n)
o = p*(p-1)//2
assert pow(f(g), k+o, n) == f(public_key)

# All that's left is to decrypt the flag
for i in range(0, 100):
    private_key = long_to_bytes(k + i * o)
    flag = AES.new(private_key, AES.MODE_ECB).decrypt(encrypted_flag)
    if b"TetCTF" in flag:
        break

flag = flag.strip(b'\x00').decode()
print(f'[+] Flag: {flag}')
```

It's amazing how the script is so simple yet only a few could solve it during the competition.

## Flag
```
TetCTF{h0m0m0rph1sm_1s_0ur_fr13nd-mobi:*100*231199111007#}
```
