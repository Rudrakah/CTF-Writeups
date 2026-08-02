# binhexa

 **Platform**  picoCTF(CyLab)
 **Category**  General Skill
 **Difficulty**  Easy

## Challenge
The challenge provides two binary numbers and asks a series of binary operations to be performed in the given order.
The operations include multiplication, bitwise AND, left shift, right shift, addition, and bitwise OR.
After completing all six operations correctly, the final binary result must be converted to hexadecimal to obtain the flag.

## Approach
I connected to the remote service using Netcat.

nc titan.picoctf.net 60885
The server provided two binary numbers.

Binary Number 1: 00010011
Binary Number 2: 11101000
I solved each operation one by one.

### Question 1

Multiply Binary Number 1 and Binary Number 2.
00010011 × 11101000
Answer:
1000100111000

### Question 2

Perform Bitwise AND.

00010011
11101000

00000000
Answer:
00000000

### Question 3
Right shift Binary Number 2 by 1 bit.
11101000 >> 1
Answer:
1110100

### Question 4

Add Binary Number 1 and Binary Number 2.

00010011
11101000

11111011
Answer:
11111011

### Question 5
Left shift Binary Number 1 by 1 bit.
00010011 << 1
Answer:
100110

### Question 6
Perform Bitwise OR.
00010011
11101000

11111011
Answer:
11111011

## Final Step
The last binary result was:
11111011


Convert it to hexadecimal.
11111011₂ = FB₁₆

Submit:
FB
The service returned the flag.

## Terminal Output
$ nc titan.picoctf.net 60885

Binary Number 1: 00010011
Binary Number 2: 11101000

Q1 (*)  -> 1000100111000
Q2 (&)  -> 00000000
Q3 (>>) -> 1110100
Q4 (+)  -> 11111011
Q5 (<<) -> 100110
Q6 (|)  -> 11111011

Hex Result:
FB
picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_aeaf4b09}

## Flag
picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_aeaf4b09}

## Tools Used
- picoCTF WebShell
- Netcat (`nc`)
- Binary Calculator (optional)

## Key Learning
- Learned how to perform basic binary arithmetic.
- Practiced bitwise operations such as **AND (`&`)** and **OR (`|`)**.
- Understood how **left shift (`<<`)** and **right shift (`>>`)** affect binary numbers.
- Learned how to convert a binary value into hexadecimal.
