# grep 2
 general skills, 125 points

## Description
 This one is a little bit harder. Can you find the flag in /problems/grep-2_0_783d3e2c8ea2ebd3799ca6a5d28fc742/files on the shell server? Remember, grep is your friend.

## Hints
 grep [tutorial](https://ryanstutorials.net/linuxtutorial/grep.php)

## Solution
 Access the said folder using the shell on their site, once you're in there you'll see a lot of folder with even more files inside them. The idea is you have to see which of these files contains the flag.

For this, we can use the recursive function of the grep command `grep -r pico`
This directly gives us the flag along with the folder and file. The flag was in file 23 inside file5


## Flag
>picoCTF{grep_r_and_you_will_find_24c911ab}
