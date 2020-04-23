# HEEEEEEERE'S Johnny!
 category, xx points

## Description
 Okay, so we found some important looking files on a linux computer. Maybe they can be used to get a password to the process. Connect with `nc 2018shell.picoctf.com 40157`. Files can be found here: [passwd](https://2018shell.picoctf.com/static/7a017af70c0b86ab002896616376499e/passwd) [shadow](https://2018shell.picoctf.com/static/7a017af70c0b86ab002896616376499e/shadow). 

## Hints
 If at first you don't succeed, try, try again. And again. And again.
 If you're not careful these kind of problems can really "rockyou".

## Solution
 Based on the name of the challenge combined with the obvious reference to rockyou.txt in the hints, I thought to run the files through John the Ripper using the rockyou wordlist

 ```
 $ john shadow --wordlist=rockyou.txt
 $ john --show shadow
 ```

 This shows us the cracked hash is `password1`, which upon submitting to the net cat along with the username `root` gives us our flag


## Flag
>picoCTF{J0hn_1$_R1pp3d_1b25af80}
