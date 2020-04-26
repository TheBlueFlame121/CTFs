# Ezoterik
 forensics, xx points

## Description
  Inventing languages is hard. Luckily, there's plenty of them, including stupid ones.

## Hints
 You will find what you seek beyond the whitespace

## Solution
 The title and the description are both clearly talking about esoteric languages. Opening up the image in the image viewer we see some symbols scribbled across the edge.

 >+[--->++<]>+++.[->++++<]>+.----.+++++++.----[->+++<]>.------------.+[----->+<]>+.+.[->+++++<]+.------------.---[->++++<]>-.----.+++..+++++++.-----[++>---<]>.

 Me and my team immediately thought it was brainfuck, a common esolang. Trying to run the above code in an online interpretor didnt give us anything. This was just a red herring.

 The real solution was to see the data in the image. Running xxd or strings on the file shows some whitespace at the end of the file followed by some data. The hint told us about this!

 ```
 2TLEdubBbS21p7u3AUWQpj1TB98gUrgHFAiFZmbeJ8qZFb9qCUc8Qp6o86eJYkrm2NLexkSDyRYd3X9sRCRKJzoZnDtrWZKcHPxjoRaFPHfmeUyoxyyWQtiqEgdJR1WU4ywAYqRq7o55XLUgmdit6svgviN8qy72wvLvT2eWjECbqHdrKa2WjiAEvgaGxVedY8SRXXcU9JbP5Ps3RY2ieejz6DrF9NBD7mri2wrsyDs9gpVgosxnYPbwjGdmsq7GwudbqtJ7SeKgaStmygyfPast5F3ZKL9KeC2LzCeenffoZ4d4Cna7TZdkUsfdK1HNmoB46fo9jK5ENQwnWdPmZBnZ4h8uDxHpQF74rs3wPcpmch6Byu31och1cyz8JxgXkacHpTrGeAN2bEhRp8kDQpmPtj9QqaAgxTbam9hoB4mvtrRmRx5GnzzZoWW5qDxwMvgKCYWiLwtLcvjDZPNdHGbvFspFeCq7kBcTeyrjYeHxuwwwM1GpdwMdxzNiFK1jYkA4DUZRohuKxeyhBFiY9HuwD6zKf9nZMThoYwTGhAJR2d3GqVqXGsivAKLs1oBzrmH9V6vaMwAjM7Hu69TLfKHtZUThoiEDftxPJdraNxoQps3mFamNbT1U3kRdpAz5s5kq6i2jLBUjBjAdV9N8jWNqx4RgiaHTW5qqb8E6JvHgQyrVkLmMdsjoLAWaWZLRw2pQpBJehRsx1LU6wmAC1nfeLbdQxPmytaMUURBDhHVqPNxwThCzZsnA9RuKrYWGsmyTxCzVUEjvUXaU4hkoV62qn7G1TnVRiADNhRfMnxm8R2ZoSPxEhVaFyHvLweq
 ```
 
 The data is base58 encoded, decoding it gives the following:

 ```
elevator lolwat
  action main
    show 114
    show 116
    show 99
    show 112
    show 123
    show 78
    show 111
    show 116
    show 32
    show 113
    show 117
    show 105
    show 116
    show 101
    show 32
    show 110
    show 111
    show 114
    show 109
    show 97
    show 108
    show 32
    show 115
    show 116
    show 101
    show 103
    show 111
    show 95
    show 52
    show 120
    show 98
    show 98
    show 52
    show 53
    show 103
    show 121
    show 116
    show 106
    show 125
  end action
  action show num
    floor num
    outFloor
  end action
end elevator
```

 This is elevator esolang. We couldn't find any interpretors for this online, but after looking at the code, we saw that the numbers in the `show` commands were ascii! Decoding those gave us the flag


## Flag
>rtcp{Not quite normal stego_4xbb45gytj}
