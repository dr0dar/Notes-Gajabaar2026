[Inroduction-module](https://cryptohack.org/courses/intro/course_details/)

---
## General - Encoding

### ASCII:
Code: 
```
# Given list
ascii_list = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

# ASCII (American Standard Code for Information Interchange)
# In python there is `chr()` function for ascii ordinal to character and `ord()` function for character to ascii ordinal converter.

flag = ""

for num in ascii_list:
    flag += chr(num)

print(flag)
```
Output:
`crypto{ASCII_pr1nt4bl3}`

### HEX:
Code:
```
# Given
hex_string = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"

# In Python, the bytes.fromhex() function can be used to convert hex to bytes. The .hex() instance method can be called on byte strings to get the hex representation.

byte = bytes.fromhex(hex_string)
print(byte)
```
Output:
`crypto{You_will_be_working_with_hex_strings_a_lot}`

### Base64:
Code:
```
import base64
# Given
hex_string = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"

#  In Python, after importing the base64 module with import base64, you can use the base64.b64encode() function.

binary = bytes.fromhex(hex_string)
base_64 = base64.b64encode(binary)
print(base_64)
```
Output:
`crypto/Base+64+Encoding+is+Web+Safe/`

### Bytes and Big Integers:
Code:
```
# Cryptography algorithms like RSA works in integers, so how we convert messages to integers
# Best option is [message -> ascii -> hex_bytes(base16, 8-bit, 1-byte) concatinate all hex bytes to single hexdata and converted into decimal(base10)]
#  Python's PyCryptodome library implements this with the methods bytes_to_long() and long_to_bytes(). You will first have to install PyCryptodome and import it with from Crypto.Util.number import *.

from Crypto.Util.number import *

# Given
integers = 11515195063862318899931685488813747395775516287289682636499965282714637259206269

binary = long_to_bytes(integers)
print(binary)

```
Output:
`crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}`

---
## General - XOR

### XOR Starter:
Code:
```
# For longer binary we xor bit by bit 0101 ^ 0011 = 0110.
# For integer we convert decimal to binary.
# For string we convert each charater to integer (unicode). 

# The Python pwntools library has a convenient xor() function that can XOR together data of different types and lengths.

# Given the string label, XOR each character with the integer 13. Convert these integers back to a string and submit the flag as crypto{new_string}

def xor_string_with_integer(string, integer):
    flag = ""
    for c in string:
        xor_int = ord(c) ^ integer
        flag += chr(xor_int)
    return flag

print(xor_string_with_integer("label", 13))
```
Output:
`aloha` 
Flag:
`crypto{aloha}`

### XOR Properties:
Code:
```
# There are four main properties we should consider when we solve challenges using the XOR operator:
# Commutative: A ⊕ B = B ⊕ A
# Associative: A ⊕ (B ⊕ C) = (A ⊕ B) ⊕ C
# Identity: A ⊕ 0 = A
# Self-Inverse: A ⊕ A = 0 

# Given
# Below is a series of outputs where three random keys have been XOR'd together and with the flag.
# Use the above properties to undo the encryption in the final line to obtain the flag.
# KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
# KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
# KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
# FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf 

# Note: (Flag ^  key) ^ key = Flag : XOR beauty is 'xoring data by key' and again 'xoring output by same key' lead us to data.

hexs = {
        "key1" : "a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313",
        "key2_xor_key1" : "37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e",
        "key2_xor_key3" : "c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1",
        "flag_xor_key1_xor_key3_xor_key2" : "04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf"
        }

def reverse(hexs):
    
    for key in hexs: # Converting to binary from hex
        hexs[key] = bytes.fromhex(hexs[key])

    key1 = hexs["key1"]
    key2 = bytes( x ^ y for x,y in zip(hexs["key2_xor_key1"],key1))
    key3 = bytes( x ^ y for x,y in zip(hexs["key2_xor_key3"],key2))
    flag = bytes( ((x ^ y) ^ z) ^ a for x,y,z,a in zip(hexs["flag_xor_key1_xor_key3_xor_key2"],key1,key2,key3))
    print(flag)

reverse(hexs)
```
Output:
`b'crypto{x0r_i5_ass0c1at1v3}'`

### Favourite Byte:
Code:
```
# I've hidden some data using XOR with a single byte, but that byte is a secret

given = "73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d"

def data(hexs:str) ->str:
    binary = bytes.fromhex(hexs)
    for i in range(256):
        string = bytes([x ^ i for x in binary])
        if b'crypto' in string:
            print(string.decode())

data(given)
```
Output:
`crypto{0x10_15_my_f4v0ur173_by7e}`

### You either know, XOR you don\`t
Code:
```
# I've encrypted the flag with my secret key, you'll never be able to guess it. 
# Remember the flag format and how it might help you in this challenge!
# a ^ b = c , c ^ a = b , c ^ b = a

given = "0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104"

def guess(hexs: str) -> str:
    binary = bytes.fromhex(hexs)

    # In starting of flag format there is always 'crypto{'
    ffb = b'crypto{'

    key = bytes([x ^ y for x,y in zip(binary,ffb)])
    print(key.decode()) # It gives us 'myXORke' thats measn key can be 'myXORkey' lets try. 

    keyf = b'myXORkey'
    print(f"lets try key: {keyf.decode()}")

    # Or we can do 'bytes([x ^ y for x,y in zip(binary, cycle(keyf))]) , we have to import: from itertools import cycle
    flag = bytes([x ^ keyf[i % len(keyf)] for i,x in enumerate(binary)])  
    
    print(f"got flag: {flag.decode()}")


guess(given)
```
Output:
`crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}`
