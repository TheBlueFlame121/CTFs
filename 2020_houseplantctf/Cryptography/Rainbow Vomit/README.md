# Rainbow Vomit
 cryptography, 535 points

## Description
 o.O What did YOU eat for lunch?!

The flag is case insensitive.

## Hints
 Replace spaces in the flag with { or } depending on their respective places within the flag.

 Hues of Hex

 This type of encoding was invented by Josh Cramer

## Solution
 By far the most annoying problem for me. This is the HexaHue cipher. It maps a 2x3 grid of 6 different colours to a letter. Here is the given ciphertext image in a better resolution:

![https://imgur.com/6bTB6Ar.jpg](https://imgur.com/6bTB6Ar.jpg)

 I tried looking for some tools which could do this automatically but was unsucessful. Now one could write a script using pythong and OpenCV edge detection but since I was lacking experience, I thought it would be faster to convert by hand using this [site](https://www.boxentriq.com/code-breaking/hexahue). Boy did it take some time, I ended up only decoding the flag which was presnt in the last sentence.

## Flag
>rtcp{should_fl5g4_b3_st1cky_or_n0t}
