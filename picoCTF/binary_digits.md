# Binary Digits
| **Platform** | picoCTF 2026 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | Binary / 0s and 1s / Image Forensics |

## Challenge

The challenge says:

This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything meaningful from this?

The challenge provides a file containing binary digits.

The main idea is to decode the sequence of `1`s and `0`s into meaningful data.

## Step 1 – Download the File

Download the challenge file.

Check the downloaded file:

    ls -l

## Step 2 – Inspect the File

Use:

    file <filename>

The file initially contains a large amount of binary-looking data consisting mainly of `1`s and `0`s.

This suggests that the data may be encoded binary rather than being random noise.

## Step 3 – Convert Binary to Bytes

The binary digits can be grouped into 8-bit sequences.

For example:

    01001000

represents the ASCII character:

    H

The binary data can therefore be converted into bytes and then interpreted as text or binary file data.

## Step 4 – Decode the Binary Data

After converting the binary digits into bytes, the resulting data forms a JPEG image.

The decoded file can be saved as:

    decoded.jpg

Check the resulting file:

    file decoded.jpg

The output identifies it as:

    JPEG image data, JFIF standard 1.01

This confirms that the binary data successfully decoded into an image.

## Step 5 – Search the Decoded File

We can search the decoded image for readable flag text:

    strings decoded.jpg | grep -i "pico"

The flag is found inside the decoded file.

## Step 6 – Get the Flag

The recovered flag is:

    picoCTF{h1dd3n_1n_th3_b1n4ry_8e65b559}

## Why This Works

The original file is not random data.

It consists of binary digits:

    0
    1

These digits represent bytes when grouped into sets of eight.

For example:

    01001000 01100101 01101100 01101100 01101111

can be converted to:

    Hello

In this challenge, decoding the binary data produces a JPEG image.

After recovering the JPEG, normal forensic tools such as `file`, `strings`, and `exiftool` can be used to inspect it.

## Important Concepts

### Binary Encoding

Binary data uses only two values:

    0
    1

Eight binary digits form one byte.

### ASCII

ASCII maps numerical byte values to characters.

For example:

    01000001 = A
    01000010 = B
    01000011 = C

### File Identification

After decoding, use:

    file decoded.jpg

to confirm that the output is a JPEG image.

### Strings

Readable text can be extracted with:

    strings decoded.jpg

Searching specifically for the flag:

    strings decoded.jpg | grep -i "pico"

### ExifTool

Metadata can also be checked using:

    exiftool decoded.jpg

This is useful when investigating suspicious images.

## Commands Used

Check the original file:

    file <filename>

Decode the binary data into bytes:

    python3 -c 'data=open("<filename>").read().strip(); open("decoded.jpg","wb").write(bytes(int(data[i:i+8],2) for i in range(0,len(data),8)))'

Check the decoded image:

    file decoded.jpg

Search for the flag:

    strings decoded.jpg | grep -i "pico"

Check metadata:

    exiftool decoded.jpg

## One-Line Solution

    Decode the 0/1 data into bytes → recover decoded.jpg → strings decoded.jpg | grep -i "pico"

## Complete Solution

    Download the binary file
            ↓
    Inspect the file
            ↓
    Notice the data contains only 0s and 1s
            ↓
    Group the binary digits into 8-bit bytes
            ↓
    Convert binary bytes into raw data
            ↓
    Save the result as decoded.jpg
            ↓
    Verify with file decoded.jpg
            ↓
    JPEG image is recovered
            ↓
    Search the decoded image
            ↓
    strings decoded.jpg | grep -i "pico"
            ↓
    Find the flag

## Flag

    picoCTF{h1dd3n_1n_th3_b1n4ry_8e65b559}

## Tools Used

- Linux Terminal
- Python3
- `file`
- `strings`
- `grep`
- `exiftool`

## Key Learning

- Binary digits can represent bytes and file data.
- Eight binary digits make one byte.
- Binary data can be converted back into its original file format.
- The `file` command helps identify recovered files.
- `strings` can reveal readable information hidden inside binary files.
- ExifTool is useful for inspecting image metadata.
- In forensic CTFs, apparently random binary data may actually contain a complete file.
