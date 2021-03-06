# Unimplemented
## The Challenge
This was the first Crypto challenge in TetCTF 2021. Though it was pretty easy, it was cool to solve regardless. We are given an RSA scheme based on Gaussian Integers. It gives us everything including the private key and we just have to decrypt the ciphertext.

### Parameter Generation
```python=
def generate_key_pair(nbits):
    while True:
        p = getPrime((nbits + 3) // 4)
        q = getPrime((nbits + 3) // 4)
        n = (p ** 2) * (q ** 2)
        if n.bit_length() == nbits:
            return (p, q), n
```
Pretty straightforward, two primes $p$ and $q$, and a modulus $n=p^2q^2$. Nothing special here, especially since we are given $p$ and $q$

### Encryption
```python=
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


def encrypt(public_key, plaintext):
    n = public_key
    plaintext = pad(plaintext, 2 * ((n.bit_length() - 1) // 8))
    m = Complex(
        int.from_bytes(plaintext[:len(plaintext) // 2], "big"),
        int.from_bytes(plaintext[len(plaintext) // 2:], "big")
    )
    c = complex_pow(m, 65537, n)
    return (c.re.to_bytes((n.bit_length() + 7) // 8, "big")
            + c.im.to_bytes((n.bit_length() + 7) // 8, "big"))
```
This is where things get a bit interesting. First thing we notice is that the group operation being used here is complex multiplication:
<br>
$$
(a_1+ib_1)*(a_2 + ib_2) = (a_1a_2 - b_1b_2) + i(a_1b_2 + a_2b_1)
$$
<br>
The `complex_pow` is the just double and add of repeated multiplication.<br>
Inside the encryption function we see that the exponent $e=65537$, and the plaintext is the is the Gaussian Integer made by splitting the flag into two halves.

## Solution Idea
Well we are given pretty much everything here:
```python= 
private_key = (128329415694646850105527417663220454989310213490980740842294900866469518550360977403743209328130516433033852724185671092403884337579882897537139175073013,               119773890850600188123646882522766760423725010264224559311769920026142724028924588464361802945459257237815241227422748585976629359167921441645714382651911)
public_key = 236252683050532196983825794701514768601125614979892312308283919527619033977486749228418695923608569040825653688303374445536392159719426640289893369552258923597180869981053519695297428186215135878525530974780390951763007339139013157234202093279764459949020588291928614938201565110828675907781512603972957429701280916745719458738970910789383870206038035515777549907045905872280803964436193687072794878040018900969772972761081589529671158140590712503582004892067155769362463889653489918914397872964087471457070748108694165412065471040954221707557816986272311750297566993468288899523479556822418109112211944932649
ciphertext = b'\x00h\xbe\x94\x8c\xcd\xdd\x04\x80\xf4\x9d\t\xd8\x8dO\x08\xf1\xd1\xc4\xb9\xa06\xe7\xe3\xb6\xc3\x01+\xa9\xf2\xb9\xe8\x8d\xe6\xc9\x0c_#\x93\x11\xad\x0f\x90\xd3\x0b6\xb0n\x13\xea~"V6\xdbA&\x87\xfe\xa3C\xcb\x16\xae\xd9\x83\xdbU\xc6\x06\xcd\x9a\x94\xa9\xce\x15{d\x95s\xc2\xfb\x90q\xe7\x02\xa2\x081:_C\xc68\x00y\x7fj4@\xd2\xcdE\x06\x943\xbe\xbcC3\xca\x91\xb4\x0e}C\xab\xff?X\xc30u\x069:Dc\xb5\xdc\x9b0%\x98\xbd\xd9\x13\xc0\x02w\xc5\xe5:\xca\xcf\x0c\xab\xc2\x9b}\xab\xd0\xcc\xbc\x0f\x9e9\t\xf7M\xb3\xed\x86\xb5E\x8b\xbc4\xfaH\x9b4\x1c\xc4\xab\xc0\xaf\x8a5XcX\x19K\xed\x19\xe1\x1c\xd0\x1e\x97c\x9fF:L\x9d\x90p\x99\xb8\xde\xcblM\xb3\x80sSL\xe1\xa4\xd6,\x81\xd6\x9c\xf1\xbb\x9c)\xf03\x155\xc0\xd5v\x13\xd6#\xb7\x19\xdea%\xce<\x1c\xf7\xf2!;\xc1\xd7w\xd1\xc8\x8d/\xaew\xa1\x1d\xc5(\xc8\x9d\x82v\xf6j\x90A,e\xbd\xa7]\x10\x8f\xe5\xe7\x93}:\xdf1~\xec\xd0-o`\r\x96\xe4\x03\xb9E\x9fdF\xc3\xf8L\xa0\xda\xf0E[\xf7\x02\xef|*\x08\x8a5pI&\xa9i\x0fZ\xa8\xb3H\xed\xe8v\xc4H\xff\xdb\xcb\x00\xf1\xae\x9bO\x18\xd5\xd8&p\xf5\xf6\xe9\xf1\x1brs\xc2\r\xceZ\xd0\xb24\x97+\x98b\x0e\xbb\xb8.\x8dT\xe4"\xad\xe4\xa3f\xd0M\xbf\xafX\xbb"[\x99\xdap\xa5\xcfT2Wx\x87M\x7f\x99!>B[Q\x04\xf6\x03\xbc\x84\xf4\xdfj\xdd1^I\x1a\x05\x81\x91\xde9Mf*\x8e\x8d\xe64\xf8\x93\x99&yP\xcd\x00!\x82\xab\xbcy\xed\xf1\x13\xd3\x81\xeaz\xbbP>\x9a2\x8c\x08\x0es\xbc\xa9\xf6\xa3\x8c\xb0\xb9t\xd9?\x06@\xc9\x90\xb7\xa7<\x85\xeb\x1a\x88#\x1c\xc3 \xec\xc7\x94d\x99\xd6\x8e>\x06\xf8Y\xf4\x19\xcaI/hy\x18\x8e\x0e8\xf8\r\xbb\xf6\x11\xb9\x8dCWB6 '
```
If we compare it normal RSA, all that's left to do is compute the group order, ie the totient $\phi(n)$ and take it's modular inverse wrt $e$ to get the private key. Once we have the private key, the same `complex_pow` should do the decryption. Well so what is this totient when Gaussian Integers are involved?<br>
<br>
Well if we just take a look at the source of unevaluated, we can see that:
```python=
n = p*p
phi = p*(p-1)*(p+1)
```
So I think it is reasonable to assume:
```python=
n= p*p*q*q
phi = p*(p-1)*(p+1)*q*(q-1)*(q+1)
```
and that does the trick!<br>
<br>
If you are interested in more of the math behind this, you can check out [rkm's blog](https://rkm0959.tistory.com/192). He has a very good writeup with lot's of different ways to solve this.<br>

## Script
```python=
from collections import namedtuple

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
    

def unpad(data):
    assert b"\x00" in data, "incorrect padding"
    return data.split(b"\x00", 1)[1]
    

private_key = (128329415694646850105527417663220454989310213490980740842294900866469518550360977403743209328130516433033852724185671092403884337579882897537139175073013,
               119773890850600188123646882522766760423725010264224559311769920026142724028924588464361802945459257237815241227422748585976629359167921441645714382651911)
public_key = 236252683050532196983825794701514768601125614979892312308283919527619033977486749228418695923608569040825653688303374445536392159719426640289893369552258923597180869981053519695297428186215135878525530974780390951763007339139013157234202093279764459949020588291928614938201565110828675907781512603972957429701280916745719458738970910789383870206038035515777549907045905872280803964436193687072794878040018900969772972761081589529671158140590712503582004892067155769362463889653489918914397872964087471457070748108694165412065471040954221707557816986272311750297566993468288899523479556822418109112211944932649
ciphertext = b'\x00h\xbe\x94\x8c\xcd\xdd\x04\x80\xf4\x9d\t\xd8\x8dO\x08\xf1\xd1\xc4\xb9\xa06\xe7\xe3\xb6\xc3\x01+\xa9\xf2\xb9\xe8\x8d\xe6\xc9\x0c_#\x93\x11\xad\x0f\x90\xd3\x0b6\xb0n\x13\xea~"V6\xdbA&\x87\xfe\xa3C\xcb\x16\xae\xd9\x83\xdbU\xc6\x06\xcd\x9a\x94\xa9\xce\x15{d\x95s\xc2\xfb\x90q\xe7\x02\xa2\x081:_C\xc68\x00y\x7fj4@\xd2\xcdE\x06\x943\xbe\xbcC3\xca\x91\xb4\x0e}C\xab\xff?X\xc30u\x069:Dc\xb5\xdc\x9b0%\x98\xbd\xd9\x13\xc0\x02w\xc5\xe5:\xca\xcf\x0c\xab\xc2\x9b}\xab\xd0\xcc\xbc\x0f\x9e9\t\xf7M\xb3\xed\x86\xb5E\x8b\xbc4\xfaH\x9b4\x1c\xc4\xab\xc0\xaf\x8a5XcX\x19K\xed\x19\xe1\x1c\xd0\x1e\x97c\x9fF:L\x9d\x90p\x99\xb8\xde\xcblM\xb3\x80sSL\xe1\xa4\xd6,\x81\xd6\x9c\xf1\xbb\x9c)\xf03\x155\xc0\xd5v\x13\xd6#\xb7\x19\xdea%\xce<\x1c\xf7\xf2!;\xc1\xd7w\xd1\xc8\x8d/\xaew\xa1\x1d\xc5(\xc8\x9d\x82v\xf6j\x90A,e\xbd\xa7]\x10\x8f\xe5\xe7\x93}:\xdf1~\xec\xd0-o`\r\x96\xe4\x03\xb9E\x9fdF\xc3\xf8L\xa0\xda\xf0E[\xf7\x02\xef|*\x08\x8a5pI&\xa9i\x0fZ\xa8\xb3H\xed\xe8v\xc4H\xff\xdb\xcb\x00\xf1\xae\x9bO\x18\xd5\xd8&p\xf5\xf6\xe9\xf1\x1brs\xc2\r\xceZ\xd0\xb24\x97+\x98b\x0e\xbb\xb8.\x8dT\xe4"\xad\xe4\xa3f\xd0M\xbf\xafX\xbb"[\x99\xdap\xa5\xcfT2Wx\x87M\x7f\x99!>B[Q\x04\xf6\x03\xbc\x84\xf4\xdfj\xdd1^I\x1a\x05\x81\x91\xde9Mf*\x8e\x8d\xe64\xf8\x93\x99&yP\xcd\x00!\x82\xab\xbcy\xed\xf1\x13\xd3\x81\xeaz\xbbP>\x9a2\x8c\x08\x0es\xbc\xa9\xf6\xa3\x8c\xb0\xb9t\xd9?\x06@\xc9\x90\xb7\xa7<\x85\xeb\x1a\x88#\x1c\xc3 \xec\xc7\x94d\x99\xd6\x8e>\x06\xf8Y\xf4\x19\xcaI/hy\x18\x8e\x0e8\xf8\r\xbb\xf6\x11\xb9\x8dCWB6 '

n = public_key
p, q = private_key
totient = p*(p-1)*(p+1)*q*(q-1)*(q+1)
d = inverse_mod(65537, totient)
m = Complex(
    int.from_bytes(ciphertext[:len(ciphertext) // 2], "big"),
    int.from_bytes(ciphertext[len(ciphertext) // 2:], "big")
)
c = complex_pow(m, d, n)
p = (int(c.re).to_bytes((n.nbits() + 7) // 8, "big") + int(c.im).to_bytes((n.nbits() + 7) // 8, "big"))
print(p.split(b'\x00')[-1].decode())
```

## Flag
```
TetCTF{c0unt1ng_1s_n0t_4lw4ys_34sy-vina:*100*48012023578024#}
```


###### tags: `CTF` `RSA` `Gaussian Integers` `TetCTF` `2021`
