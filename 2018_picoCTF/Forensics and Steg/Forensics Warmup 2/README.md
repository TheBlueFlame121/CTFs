# Forensics Warmup 2
 Forensics, 50 points

## Description
 Hmm for some reason I can't open this [PNG](https://2018shell.picoctf.com/static/0fde1a3e09824365348827194db9cdde/flag.png)? Any ideas? 

## Hints
 How do operating systems know what kind of file it is? (It's not just the ending!

## Solution
 After trying to open the file with an image viewer, we get an error. So I decided to run the file command on it, this was the output:
 ```
 file flag.png 
 flag.png: JPEG image data, JFIF standard 1.01, resolution (DPI), density 75x75, segment length 16, baseline, precision 8, 909x190, frames 3
 ```

 This means that the file infact is a jpg and not a png. Renaming:
 `mv flag.png flag.jpg`

 Now it opens like a normal image to give us the flag.
 ![https://i.imgur.com/hDhIvR5.jpg](https://i.imgur.com/hDhIvR5.jpg)


## Flag
>picoCTF{extensions_are_a_lie}
