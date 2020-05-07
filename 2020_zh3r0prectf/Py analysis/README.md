# Py analysis
 cryptography, 270 points

## Description
One layer of encryption may be easy to crack, but multi-layers need not be easy. Do ping us in discord if you have any issues, Hint: the file checks the flag entered and encrypts it and compares it to a result, so the first thought is the using the given result I the file reverse the process of encryption and get the flag

## Solution
Given was the following python file: 

```python


from Crypto.Util.number import bytes_to_long, long_to_bytes
import base64


# file=open('flag1.txt','r')
# flag=file.read()

# print(flag)   # ;p
print('Enter the flag and verify it here \nGood Luck')
def encrypt1(flag):
    
    enc1=str(bytes_to_long(flag.encode()))
    
    print('Encryption 1 complete')
    
    return enc1 

def encrypt2(flag):
    
    enc2=[]
    
    for byte in range(len(flag)):
        res=str(int(flag[byte])<<byte)
        enc2.append(res)
    print('Encryption 2 complete')
    return enc2
 
    
def final_encrypt(flags):
    res=[]
    for flag in flags:
        r=base64.b64encode(flag.encode()).decode().rstrip('=')
        
        res.append(r)
    print('Final encryption done.')
    return res
def flagenc(flag_input):
    
    flag=flag_input
    
    if len(flag)!=34:
        print('Wrong length')
        return ' '
       
    enc1=encrypt1(flag)
    
    enc2=encrypt2(enc1)

    final_enc=final_encrypt(enc2)
    
    return final_enc

enc_flag=['Mw', 'MTI', 'OA', 'NjQ', 'NjQ', 'MjU2', 'MjU2', 'MTI4', 'MTc5Mg', 'NDA5Ng', 'MA', 'MTAyNDA', 'MA', 'NzM3Mjg', 'MTQ3NDU2', 'MA', 'NjU1MzY', 'MTE3OTY0OA', 'MTU3Mjg2NA', 'MTU3Mjg2NA', 'ODM4ODYwOA', 'MTY3NzcyMTY', 'MTI1ODI5MTI', 'ODM4ODYwOA', 'MTUwOTk0OTQ0', 'MTAwNjYzMjk2', 'MTM0MjE3NzI4', 'MTIwNzk1OTU1Mg', 'ODA1MzA2MzY4', 'MzIyMTIyNTQ3Mg', 'MTA3Mzc0MTgyNA', 'MjE0NzQ4MzY0OA', 'MzAwNjQ3NzEwNzI', 'MTcxNzk4NjkxODQ', 'MA', 'MzA5MjM3NjQ1MzEy', 'MTM3NDM4OTUzNDcy', 'ODI0NjMzNzIwODMy', 'MTA5OTUxMTYyNzc3Ng', 'NTQ5NzU1ODEzODg4', 'OTg5NTYwNDY0OTk4NA', 'MTUzOTMxNjI3ODg4NjQ', 'MTMxOTQxMzk1MzMzMTI', 'NTI3NzY1NTgxMzMyNDg', 'MTIzMTQ1MzAyMzEwOTEy', 'NzAzNjg3NDQxNzc2NjQ', 'MjgxNDc0OTc2NzEwNjU2', 'ODQ0NDI0OTMwMTMxOTY4', 'MA', 'MTY4ODg0OTg2MDI2MzkzNg', 'MTAxMzMwOTkxNjE1ODM2MTY', 'MTgwMTQzOTg1MDk0ODE5ODQ', 'MzYwMjg3OTcwMTg5NjM5Njg', 'ODEwNjQ3OTMyOTI2Njg5Mjg', 'MTQ0MTE1MTg4MDc1ODU1ODcy', 'MTgwMTQzOTg1MDk0ODE5ODQw', 'NzIwNTc1OTQwMzc5Mjc5MzY', 'MTQ0MTE1MTg4MDc1ODU1ODcy', 'MjMwNTg0MzAwOTIxMzY5Mzk1Mg', 'NDYxMTY4NjAxODQyNzM4NzkwNA', 'MjMwNTg0MzAwOTIxMzY5Mzk1Mg', 'MTE1MjkyMTUwNDYwNjg0Njk3NjA', 'NDE1MDUxNzQxNjU4NDY0OTExMzY', 'MTg0NDY3NDQwNzM3MDk1NTE2MTY', 'MTY2MDIwNjk2NjYzMzg1OTY0NTQ0', 'MzY4OTM0ODgxNDc0MTkxMDMyMzI', 'NjY0MDgyNzg2NjUzNTQzODU4MTc2', 'MA', 'MTE4MDU5MTYyMDcxNzQxMTMwMzQyNA', 'NDcyMjM2NjQ4Mjg2OTY0NTIxMzY5Ng', 'MjM2MTE4MzI0MTQzNDgyMjYwNjg0OA', 'MA', 'NDcyMjM2NjQ4Mjg2OTY0NTIxMzY5Ng', 'ODUwMDI1OTY2OTE2NTM2MTM4NDY1Mjg', 'NTY2NjgzOTc3OTQ0MzU3NDI1NjQzNTI', 'MTEzMzM2Nzk1NTg4ODcxNDg1MTI4NzA0', 'MzAyMjMxNDU0OTAzNjU3MjkzNjc2NTQ0', 'NjA0NDYyOTA5ODA3MzE0NTg3MzUzMDg4', 'MjcyMDA4MzA5NDEzMjkxNTY0MzA4ODg5Ng', 'MA', 'OTY3MTQwNjU1NjkxNzAzMzM5NzY0OTQwOA', 'MTIwODkyNTgxOTYxNDYyOTE3NDcwNjE3NjA']


flag_input=input('Enter the flag:')

flag=flagenc(flag_input)


for i in range(len(flag)):
    if flag[i]!=enc_flag[i]:
        print('Wrong flag')
        print('Try more harder!!!')
        break
else:
    print('Great the flag is right go submit it')
    
```

Since the output and the functions were given to us, it was easy to create the following reverse script

```python
from Crypto.Util.number import bytes_to_long, long_to_bytes
import base64


def decrypt1(enc):
    return long_to_bytes(int(enc)).decode()


def decrypt2(flags):
    dec = ''
    for i in range(len(flags)):
        res = str(int(flags[i]) >> i)
        dec += res
    return dec


def final_decrypt(flags):
    res = []
    for flag in flags:
        r = base64.b64decode((flag + '==').encode()).decode()
        res.append(r)
    print('Final decryption done.')
    return res


def flagdec(enc_output):
    a = enc_output

    b = final_decrypt(a)
    c = decrypt2(b)
    d = decrypt1(c)

    return d


enc_flag = ['Mw', 'MTI', 'OA', 'NjQ', 'NjQ', 'MjU2', 'MjU2', 'MTI4', 'MTc5Mg', 'NDA5Ng', 'MA', 'MTAyNDA', 'MA', 'NzM3Mjg', 'MTQ3NDU2', 'MA', 'NjU1MzY', 'MTE3OTY0OA', 'MTU3Mjg2NA', 'MTU3Mjg2NA', 'ODM4ODYwOA', 'MTY3NzcyMTY', 'MTI1ODI5MTI', 'ODM4ODYwOA', 'MTUwOTk0OTQ0', 'MTAwNjYzMjk2', 'MTM0MjE3NzI4', 'MTIwNzk1OTU1Mg', 'ODA1MzA2MzY4', 'MzIyMTIyNTQ3Mg', 'MTA3Mzc0MTgyNA', 'MjE0NzQ4MzY0OA', 'MzAwNjQ3NzEwNzI', 'MTcxNzk4NjkxODQ', 'MA', 'MzA5MjM3NjQ1MzEy', 'MTM3NDM4OTUzNDcy', 'ODI0NjMzNzIwODMy', 'MTA5OTUxMTYyNzc3Ng', 'NTQ5NzU1ODEzODg4', 'OTg5NTYwNDY0OTk4NA', 'MTUzOTMxNjI3ODg4NjQ', 'MTMxOTQxMzk1MzMzMTI', 'NTI3NzY1NTgxMzMyNDg', 'MTIzMTQ1MzAyMzEwOTEy', 'NzAzNjg3NDQxNzc2NjQ', 'MjgxNDc0OTc2NzEwNjU2', 'ODQ0NDI0OTMwMTMxOTY4', 'MA', 'MTY4ODg0OTg2MDI2MzkzNg', 'MTAxMzMwOTkxNjE1ODM2MTY', 'MTgwMTQzOTg1MDk0ODE5ODQ', 'MzYwMjg3OTcwMTg5NjM5Njg', 'ODEwNjQ3OTMyOTI2Njg5Mjg', 'MTQ0MTE1MTg4MDc1ODU1ODcy', 'MTgwMTQzOTg1MDk0ODE5ODQw', 'NzIwNTc1OTQwMzc5Mjc5MzY', 'MTQ0MTE1MTg4MDc1ODU1ODcy', 'MjMwNTg0MzAwOTIxMzY5Mzk1Mg', 'NDYxMTY4NjAxODQyNzM4NzkwNA', 'MjMwNTg0MzAwOTIxMzY5Mzk1Mg', 'MTE1MjkyMTUwNDYwNjg0Njk3NjA', 'NDE1MDUxNzQxNjU4NDY0OTExMzY', 'MTg0NDY3NDQwNzM3MDk1NTE2MTY', 'MTY2MDIwNjk2NjYzMzg1OTY0NTQ0', 'MzY4OTM0ODgxNDc0MTkxMDMyMzI', 'NjY0MDgyNzg2NjUzNTQzODU4MTc2', 'MA', 'MTE4MDU5MTYyMDcxNzQxMTMwMzQyNA', 'NDcyMjM2NjQ4Mjg2OTY0NTIxMzY5Ng', 'MjM2MTE4MzI0MTQzNDgyMjYwNjg0OA', 'MA', 'NDcyMjM2NjQ4Mjg2OTY0NTIxMzY5Ng', 'ODUwMDI1OTY2OTE2NTM2MTM4NDY1Mjg', 'NTY2NjgzOTc3OTQ0MzU3NDI1NjQzNTI', 'MTEzMzM2Nzk1NTg4ODcxNDg1MTI4NzA0', 'MzAyMjMxNDU0OTAzNjU3MjkzNjc2NTQ0', 'NjA0NDYyOTA5ODA3MzE0NTg3MzUzMDg4', 'MjcyMDA4MzA5NDEzMjkxNTY0MzA4ODg5Ng', 'MA', 'OTY3MTQwNjU1NjkxNzAzMzM5NzY0OTQwOA', 'MTIwODkyNTgxOTYxNDYyOTE3NDcwNjE3NjA']

print(flagdec(enc_flag))
```


## Flag
>zh3r0{A55a55in's_Cr33d_B14ck_F14g}
