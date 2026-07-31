# interencdec

**Platform**  picoCTF(CyLab)
**Category** Cryptography 
**Difficulty**  Easy 

## Challenge
The challenge provides a file named **enc_flag** and asks us to recover the real meaning hidden inside it.
The only hint given is:
> Engaging in various decoding processes is of utmost importance.
This suggests that the flag has been encoded multiple times using different encoding techniques.

## Cipher Analysis
The contents of `enc_flag` looked like Base64 because it ended with `==`.
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg==
The decoding process was:
1. Decode **Base64** once.
2. The result was a Python byte string:
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzc4MjUwaG1qfQ=='
3. Remove the `b' '` wrapper.
4. Decode the remaining Base64 string again.
After the second Base64 decode:
wpjvJAM{jhlzhy_k3jy9wa3k_78250hmj}
This text still wasn't readable, indicating one more layer of encoding.
The challenge tags included **caesar**, so I applied a **Caesar Cipher** with a shift of **7**.

## Approach
The final decoding step revealed the original flag
picoCTF{caesar_d3cr9pt3d_78250afc}

## Flag
picoCTF{caesar_d3cr9pt3d_78250afc}

## Tools Used
- CyberChef
- Base64 Decoder
- Caesar Cipher Decoder

## Key Learning
- A challenge may use multiple layers of encoding.
- Base64 is an encoding format, not encryption.
- Always inspect the output after each decoding step before deciding the next technique.
- Challenge tags and hints can provide useful clues about the remaining encoding layers.
