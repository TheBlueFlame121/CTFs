# Strings
 general skills, 100 points

## Description
  Can you find the flag in this [file](https://2018shell.picoctf.com/static/e78981e684a62559baaef12a27f0e918/strings) without actually running it? You can also find the file in /problems/strings_0_bf57524acf558aca2081eb97ece8e2ee on the shell server. 

## Hints
 [strings](https://linux.die.net/man/1/strings)

## Solution
 Just run the strings command on the file given `strings strings| grep pico'


## Flag
>picoCTF{sTrIngS_sAVeS_Time_c09b1444}
