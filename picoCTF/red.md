# RED
| **Platform** | picoCTF 2025 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Shuailin Pan (LeConjuror) |
| **Topic** | Image Forensics / Steganography / LSB |

## Challenge

The challenge says:

```text
RED, RED, RED, RED
```

Download the image:

```text
red.png
```

Hints:

```text
The picture seems pure, but is it though?
Red?Ged?Bed?Aed?
Check whatever Facebook is called now.
```

The image looks completely red, but hidden information is stored inside its image channels.

## Step 1 – Download the Image

Download `red.png` from the challenge.

Check the file:

```bash
ls -l red.png
```

Identify the file type:

```bash
file red.png
```

## Step 2 – Inspect the Image

Use `strings` to search for hidden readable information:

```bash
strings red.png
```

The extracted information gives the clue:

```text
CHECKLSB
```

This tells us to inspect the LSB (Least Significant Bit).

## Step 3 – Understand the Hints

The hint:

```text
Red?Ged?Bed?Aed?
```

points to the four image channels:

```text
R = Red
G = Green
B = Blue
A = Alpha
```

Therefore, the LSB of the RGBA channels needs to be examined.

The hint about Facebook points to:

```text
Meta
```

which provides another clue toward examining hidden image information.

## Step 4 – Extract the LSB Data

Create a Python script:

```python
from PIL import Image

img = Image.open("red.png")
pixels = list(img.getdata())

binary = ""

for pixel in pixels:
    for channel in pixel:
        binary += str(channel & 1)

result = ""

for i in range(0, len(binary), 8):
    byte = binary[i:i+8]
    if len(byte) == 8:
        result += chr(int(byte, 2))

print(result)
```

Save it as:

```text
extract_lsb.py
```

Run:

```bash
python3 extract_lsb.py
```

The output contains encoded data.

## Step 5 – Decode the Base64 Data

The extracted data is Base64 encoded.

Decode it using:

```bash
echo "BASE64_STRING" | base64 -d
```

You can also use CyberChef with:

```text
From Base64
```

## Step 6 – Get the Flag

The decoded result gives:

```text
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

## Why This Works

The image looks completely red, but information is hidden in the least significant bits of its RGBA channels.

The attack path is:

```text
red.png
   ↓
strings red.png
   ↓
CHECKLSB
   ↓
RGBA channels
   ↓
Extract LSB
   ↓
Encoded data
   ↓
Base64 decode
   ↓
Flag
```

## Important Concepts

### LSB Steganography

LSB means Least Significant Bit.

For example:

```text
10010110
```

The LSB is:

```text
0
```

Changing only the LSB causes a very small change to a pixel value, making it useful for hiding information.

### RGBA Channels

An image can contain four channels:

```text
R = Red
G = Green
B = Blue
A = Alpha
```

The challenge uses these channels to hide data.

### Base64

Base64 is an encoding technique. It can be decoded using:

```bash
base64 -d
```

or CyberChef.

## Commands Used

```bash
file red.png
```

```bash
strings red.png
```

```bash
python3 extract_lsb.py
```

```bash
echo "BASE64_STRING" | base64 -d
```

## One-Line Solution

```bash
python3 extract_lsb.py | base64 -d
```

## Complete Solution

```text
Download red.png
        ↓
strings red.png
        ↓
Find CHECKLSB
        ↓
Identify RGBA channels
        ↓
Extract LSB from R,G,B,A
        ↓
Obtain encoded data
        ↓
Decode Base64
        ↓
Get the flag
```

## Flag

```text
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

## Tools Used

- Linux Terminal
- Python
- Pillow
- `file`
- `strings`
- `base64`
- CyberChef
- LSB extraction

## Key Learning

- Images can contain hidden information inside pixel values.
- LSB is commonly used for image steganography.
- RGBA images contain four separate channels.
- `strings` can reveal hidden clues inside binary files.
- Base64 is encoding, not encryption.
- A normal-looking image can contain hidden information.
- CTF hints often point directly toward the intended technique.

## Final Solution

```text
red.png
   ↓
strings red.png
   ↓
CHECKLSB
   ↓
RGBA
   ↓
Extract LSB
   ↓
Base64
   ↓
Decode
   ↓
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```
