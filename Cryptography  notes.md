** #01 solution for RSA encryption for give n,c,e,phi **

```python
#pip install pycrypto
#!/bin/python3

from Crypto.Util.number import * 

n = 
e =  
c =  
phi = 

d = inverse(e,phi) 
m = pow(c,d,n) 

print(long_to_bytes(m))

```

** #02 solution for RSA encryption give n,c,e (n have factors) **

```python
#!/venv/bin/python2 
#pip install pycrypto

from Crypto.Util.number import inverse 
#n,c,e from challenge
n = 
e = 
c = 

# p and q from factor db 
p = 
q = 

phi = (p-1) *(q-1)

d = inverse(e,phi)

m=pow(c,d,n)

print hex(m)[2:-1].decode('hex')    

```

** #03 solution for RSA encryption give n,c,e (n cant factorize from factor db) after this use #02 **

```bash 

sudo apt install -y build-essential libgmp3-dev zlib1g-dev libecm-dev

wget https://jaist.dl.sourceforge.net/project/msieve/msieve/Msieve%20v1.53/msieve153_src.tar.gz

tar xvf msieve153_src.tar.gz

cd msieve-1.53 

make all ECM=1

./msieve -q 'n value'

#will get p and q as a output of this tool 

```

** #04 solution for RSA encryption give n,c,e small e attack (n cant factorize from factor db) **

```python 
#!/venv/bin/python3
#pip3 install gmpy
#pip3 install pycrypto

import gmpy
from Crypto.Util.number import *
#e value must be small like 3 
#no need of n here 
e = 
c = 

m = long_to_bytes(gmpy.root(c,e)[0])
print(m)
```


** #05 solution for 4 rotor enigma machine brutefoce **

```python 
#pip install py-enigma
#!/bin/python3

from enigma.machine import EnigmaMachine 

reflectors = ['B-Thin', 'C-Thin'] 
rotors = ['I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII'] 

for r1 in rotors: 
    for r2 in rotors: 
        for r3 in rotors: 
            for r in reflectors: 
                machine = EnigmaMachine.from_key_sheet( 
                   rotors=' '.join(['Gamma', r1, r2, r3]), 
                   reflector=r, 
                   ring_settings='E J D T', 
                   plugboard_settings='fv cd hu ik es op yl wq jm'.upper()) 
                machine.set_display('CFLG') 
                temp = machine.process_text('zybmleyxesrighpyidtvsewqknwejcxzxkwwlxkceesnxhr') 
                if 'FLAG' in temp: 
                    print(temp, r1, r2, r3, r) 

```

** #06 solution for Vigenère Cipher key brutefoce using wordlist **

```python
#!/bin/python3

LETTERS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def translateMessage(message,key):
    
    mode = 'decrypt'
    translated = [] 

    keyIndex = 0
    key = key.upper()

    for symbol in message: 
        num = LETTERS.find(symbol.upper())
        if num != -1:  
            if mode == 'encrypt':
                num += LETTERS.find(key[keyIndex])
            elif mode == 'decrypt':
                num -= LETTERS.find(key[keyIndex])

            num %= len(LETTERS)

            if symbol.isupper():
                translated.append(LETTERS[num])
            elif symbol.islower():
                translated.append(LETTERS[num].lower())

            keyIndex += 1  
            if keyIndex == len(key):
                keyIndex = 0
        else:
            translated.append(symbol)
    plain = ''.join(translated)
    if "DotCTF" in plain:
        print(plain)
    

def main():
    file1 = open('/usr/share/seclists/Passwords/darkc0de.txt', 'r')
    Lines = file1.readlines()
    count = 0
    message = input("Enter the cipher text : ")
    for line in Lines:
        count += 1
        translateMessage(message,line.strip())
try:
    main()
except Exception:
    pass
```




