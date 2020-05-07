# BaseXor
 cryptography, 481 points

## Description
The author seemed to find a strange string can u help him in decoding them

FQ0ACgAUVgcCSTAdAwpvGA0AFm8EVkonAVo6BkhCWBg=


## Solution
 As suggested by the challenge title, it is a base change and an XOR encryption. So I decoded base 64, and then to decrypt the XOR, I used a neat little trick. The key is unknown, but the start of the flag `zh3r0` is known. So XOR the ciphertext with zh3r0 to get the key, then use the key to decrypt the entire thing.

 The key was `oe3x0`


## Flag
>zh3r0{34zy_x0r_wh3n_k3y_15_50r7}
