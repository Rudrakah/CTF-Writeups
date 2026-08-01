# endianness
 **Platform**  picoCTF 2024 
 **Category**  General Skills 
 **Difficulty**  Easy 

## Challenge
The challenge provides a remote service that asks for the **Little Endian** and **Big Endian** hexadecimal representations of a given word.
The objective is to correctly convert the given word into both endian formats. If both answers are correct, the service reveals the flag.
The hints suggest referring to the ASCII table and understanding how endianness works before attempting the challenge.

## Approach
I connected to the remote service using the provided Netcat command.
nc titan.picoctf.net 49382
The server provided the following word:
rtpnz

### Step 1: Convert each character to hexadecimal
Using the ASCII table:
 Character | Hex 
 r  72 
 t  74 
 p  70 
 n  6e 
 z  7a 
So the hexadecimal representation becomes:
72 74 70 6e 7a

### Step 2: Little Endian
Little Endian stores the bytes in reverse order.
72 74 70 6e 7a
        ↓
7a 6e 70 74 72
Little Endian representation:
7a6e707472

### Step 3: Big Endian
Big Endian stores the bytes in the original order.
7274706e7a
After submitting both answers, the service returned the flag.

## Terminal Output
$ nc titan.picoctf.net 49382
Word: rtpnz

Enter the Little Endian representation:
7a6e707472
Correct Little Endian representation!

Enter the Big Endian representation:
7274706e7a
Correct Big Endian representation!

Your Flag is:
picoCTF{3ndi4n_sw4p_su33ess_28329f0a}

## Flag
picoCTF{3ndi4n_sw4p_su33ess_28329f0a}

## Tools Used
- picoCTF WebShell
- Netcat (`nc`)
- ASCII Table

## Key Learning
- Learned the difference between **Little Endian** and **Big Endian** byte ordering.
- Practiced converting ASCII characters into hexadecimal values.
- Understood how byte order changes while the individual byte values remain the same.
- Learned how endianness is commonly used in computer architecture and binary data representation.
