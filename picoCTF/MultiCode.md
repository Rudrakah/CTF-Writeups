# MultiCode

| **Platform** | picoCTF(CyLab) |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge provides an encoded message containing the flag. The hint mentions that the message has been wrapped in multiple layers of common encodings, including **ROT13**, **URL Encoding**, **Hex**, and **Base64**.
The goal is to determine the correct decoding order and recover the original flag.

## Approach
I downloaded the provided message and opened it in **CyberChef**.
Since the hint indicated that multiple encoding techniques were used, I inspected the encoded text and decoded it layer by layer.
The decoding sequence was:

```text
Base64
↓
Hex
↓
URL Decode
↓
ROT13
```
After applying each decoding step in the correct order, the encoded message was converted back into plain text, revealing the flag

## Tools Used
- CyberChef
- Base64 Decoder
- From Hex
- URL Decode
- ROT13
  
## Flag
```text
picoCTF{nested_enc0ding_ffbbbf57}
```

## Key Learning
- A message can be encoded multiple times using different encoding techniques.
- Correctly identifying the encoding order is essential to recover the original data.
- CyberChef is an excellent tool for analyzing and decoding layered encodings.
