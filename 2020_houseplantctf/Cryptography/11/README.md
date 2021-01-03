# 11
 cryptography, 1527 points

## Description
 I wrote a quick script, would you like to read it? - Delphine

(doorbell rings)
delphine: Jess, I heard you've been stressed, you should know I'm always ready to help!
Jess: Did you make something? I'm hungry...
Delphine: Of course! Fresh from the bakery, I wanted to give you something, after all, you do so much to help me all the time!
Jess: Aww, thank you, Delphine! Wow, this bread smells good. How is the bakery?
Delphine: Lots of customers and positive reviews, all thanks to the mention in rtcp!
Jess: I am really glad it's going well! During the weekend, I will go see you guys. You know how much I really love your amazing black forest cakes.
Delphine: Well, you know that you can get a free slice anytime you want.
(doorbell rings again)
Jess: Oh, that must be Vihan, we're discussing some important details for rtcp.
Delphine: sounds good, I need to get back to the bakery!
Jess: Thank you for the bread! <3

## Hints
 I was eleven when I finished A Series of Unfortunate Events.

 Flag is in format: rtcp{.*}

## Solution
 This problem is based on the Seabald cipher from the aforementioned 'A Series of Unfortunate Events'. Though it is closer to steg than crypto in my opinion, the Seabald cipher work likes this: The start of the cipher is signified by the ring of a bell or the mention of a bell ringing, the secret is uncovered then by taking every 11th word (including the first one) after that till another bell rings or is mentioned ringing.

 Following that, we can uncover the secret as:`Im hungry give me bread and I will love you`

## Flag
>rtcp{Im_hungry_give_me_bread_and_I_will_love_you}
