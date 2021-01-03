# Parasite
 cryptography, 784 points

## Description
 paraSite Killed me A liTtle inSide

Flag: English, case insensitive; turn all spaces into underscores

## Hints
 Make sure you keep track of the spacing- it's there for a reason

## Solution
 The file given contains what appears to be morse, decoding it gives:

>jdu mek kdf puf ptk jef lu geuk chk kuw fu be

Okay, now what? After a bit of head-scratching I noticed the peculiar capitalisation in the description. It says SKATS, googling which lead to this [wiki page](https://en.wikipedia.org/wiki/SKATS)

Since there was no tool to convert SKATS to korean langul automatically, I did it manually using the conversion table and an online korean keyboard. We get the following langul

>희 망 은 진 정 한 기 생 충 입 니 다

which translates to `Hope is a true parasite.`

Note: in the SKATS `GEUK`, `EU` is a single letter

## Flag
>rtcp{hope_is_a_true_parasite}

