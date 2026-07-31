# The Numbers
**Platform:** picoCTF(Cylab)
**Category:** Cryptography
**Difficulty:** Easy

## Challenge
The challenge provides an image named **numbers.png** containing a sequence of numbers instead of plain text.
The objective is to determine what the numbers represent and recover the hidden flag.
The hint states:
> The flag is in the format `PICOCTF{}`.

## Approach
After opening **numbers.png**, I noticed that it contained only numbers separated by spaces.
```
16 9 3 15 3 20 6 { 20 8 5
14 21 13 2 5 18 19 13 1
19 15 14 }
```
The values ranged from **1 to 26**, which suggested they might represent the positions of letters in the English alphabet.
Using the mapping:
| Number | Letter |
|-------:|:------:|
| 1 | A |
| 2 | B |
| 3 | C |
| ... | ... |
| 16 | P |
| 20 | T |
| 26 | Z |
I converted each number into its corresponding letter.
16 → P
9  → I
3  → C
15 → O
3  → C
20 → T
6  → F
Applying the same conversion to the remaining numbers produced the complete flag.

## Decoded Flag
PICOCTF{THENUMBERSMASON}

## Tools Used
- Image Viewer
- Alphabet (A1Z26) Cipher

## Key Learning

- Learned how the **A1Z26 cipher** maps numbers to alphabet positions.
- Recognized that values from **1–26** often indicate alphabetical substitution.
- Simple pattern recognition can quickly solve beginner cryptography challenges.
