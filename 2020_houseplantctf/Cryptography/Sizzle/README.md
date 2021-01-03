# Sizzle
 cryptography, 50 points

## Description
 Due to the COVID-19 outbreak, we ran all out of bacon, so we had to use up the old stuff instead. Sorry for any inconvenience caused...

## Hints
 Wrap your flag with rtcp{}, use all lowercase, and separate words with underscores.

 Is this really what you think it is?

## Solution
 This problem was very simple actually. The contents of encoded are as follows:

>....- ..... ...-. .--.- .--.. ....- -..-- -..-. ..--. -.... .-... .-.-. .-.-. ..-.. ...-- ..... .--.. ...-- .-.-- .--.- -.... -...- .-... ..-.- .-... ..-.. ...--

 At first this seems like morse code but that isn't the case which is easily confirmed by decoding it. Notice the use of the word bacon in the description, also the second hint. This is actually just bacon cipher with different letters

![https://imgur.com/iaHvXK9.jpg](https://imgur.com/iaHvXK9)


## Flag
>rtcp{bacon_but_grilled_and_morsified}
