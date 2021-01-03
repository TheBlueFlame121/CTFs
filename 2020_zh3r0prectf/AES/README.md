# AES
 cryptography, 567 points

## Description
 You people kept asking about AES. So here it is

0d152c0ffefd78da21f04c769546b29b

enc(Key)="dnmjrqsqfsfnfqgt"

## Hints
no 13 may help you

## Solution
 So here we have a hex, that is the flag after AES encryption. Also given is the key after some encryption, the hint mentioning 13 tells us it might be ROT13 encrypted.

```
ROT13(key) = "qazwedfdsfsasdtg"
```

Since we have the key now, the only thing we have to do is AES decrypt which is trivial, here is a python script for that:

```python
from Cryptodome.Cipher import AES

k = 'qazwedfdsfsasdtg'.encode()
c = bytes.fromhex('0d152c0ffefd78da21f04c769546b29b')
cipher = AES.new(k, AES.MODE_ECB)
p = cipher.decrypt(c)
print(p.decode())
```


## Flag
>zh3r0{AES_1s_C00000l}
