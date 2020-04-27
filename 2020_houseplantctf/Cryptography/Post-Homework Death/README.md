# Post-Homework Death
 cryptoghaphy, 570 points

## Description
My math teacher made me do this, so now I'm forcing you to do this too.

Flag is all lowercase; replace spaces with underscores.

## Hints
 When placing the string in the matrix, go up to down rather than left to right.

 Google matrix multiplication properties if you're stuck.

## Solution
 This cipher is like a part of the [Hill cipher](https://en.wikipedia.org/wiki/Hill_cipher)

 We are given the decoding matrix and the encoded string, all we have to do is take the numbers in the encoded string three at a time, form a 3x1 matrix and multiply it with the decoding matrix. This gives us multiple 3x1 matrices, we just take the numbers inside them

>7 15 0 4 15 0 25 15 21 18 0 8 15 13 5 23 15 18 11 0 0

 This is an A1Z26 cipher with the 0 as space

## Flag
>rtcp{go_do_your_homework}
