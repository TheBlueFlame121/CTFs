# Reversing Warmup 2
 reversing, 50 points

## Description
 Can you decode the following string `dGg0dF93NHNfczFtcEwz` from base64 format to ASCII?

## Hints
 Submit your answer in our competition's flag format. For example, if you answer was 'hello', you would submit 'picoCTF{hello}' as the flag.

## Solution
 we put the string in the standard output and then decode it.

 `echo "dGg0dF93NHNfczFtcEwz" | base64 -d`


## Flag
>picoCTF{th4t_w4s_s1mpL3}
