# Crypto Warmup 1
 cryptography, 75 points

## Description
 Crpyto can often be done by hand, here's a message you got from a friend, `llkjmlmpadkkc` with the key of `thisisalilkey`. Can you use this [table](https://2018shell.picoctf.com/static/7e80900bd1afae76845553d895e271e1/table.txt) to solve it?. 

## Hints
 Submit your answer in our competition's flag format. For example, if you answer was 'hello', you would submit 'picoCTF{HELLO}' as the flag
 Please use all caps for the message

## Solution
 Looking at the table, this seems like a simple vernam cipher. The letter at each position is rotated by the value given by the letter at each position in the key.
 
 dcode.fr/en has a tool for this if you're lazy


## Flag
>picoCTF(SECRETMESSAGE}
