# repetitions

 **Platform**  picoCTF(CyLab)
 **Category**  General Skills 
 **Difficulty**  Easy 

## Challenge
The challenge provides an encoded file named `enc_flag`.
The file contains Base64-encoded text, but decoding it only once does not reveal the flag. The hint suggests that multiple decoding operations are required.

## Encoded File
The downloaded file contained a long Base64 encoded string.
VmpGU1EyRXlUWGxTYmxKVVYwZFNWbGxyV21GV1JteDBUbF...

## Approach
The challenge hint says:
> **Multiple decoding is always good.**
I opened the encoded file in **CyberChef** and selected the **From Base64** recipe.
After decoding once, the output was still another Base64 string instead of readable text.
I repeatedly applied **From Base64** until the output became plain text.
After several decoding rounds, the final output revealed the flag.

## Steps
1. Download the `enc_flag` file.
2. Open the file in CyberChef.
3. Add the **From Base64** recipe.
4. Decode the output repeatedly.
5. Continue until readable text appears.
6. The final output is the flag.

## Flag
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_dfe803c6}

## Tools Used
- CyberChef
- Base64 Decoder

## Key Learning
- Base64 encoding can be applied multiple times.
- If the decoded output still looks like Base64, decode it again.
- CyberChef makes repeated decoding very easy using multiple **From Base64** operations.
