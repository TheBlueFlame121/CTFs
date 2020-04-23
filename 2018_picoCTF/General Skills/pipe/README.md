# pipe
 general skilla, 110 points

## Description
 During your adventure, you will likely encounter a situation where you need to process data that you receive over the network rather than through a file. Can you find a way to save the output from this program and search for the flag? Connect with `2018shell.picoctf.com 48696`. 

## Hints
 Ever heard of a pipe? No not that kind of pipe... This [kind](http://www.linfo.org/pipes.html)

## Solution
 `nc 2018shell.picoctf.com 48696 | grep pico`


## Flag
>picoCTF{almost_like_mario_f617d1d7}

