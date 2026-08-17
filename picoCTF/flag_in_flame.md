# Flag in Flame
| **Platform** | picoMini by CMIU-Africa |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Prince Nyonshuti N. |
| **Topic** | Base64 / Image Forensics |

## Challenge

The challenge says:

The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. The goal is to inspect the file and reveal the hidden information.

The challenge provides encoded log data.

The hint says:

    Use base64 to decode the data and generate the image file.

## Step 1 – Download the Encoded Data

Download the `Logs Data` file from the challenge.

Check the downloaded file:

    ls -lh

## Step 2 – Identify the Encoding

The challenge contains a large block of encoded text.

The hint tells us that the data is encoded using Base64.

Base64 commonly contains characters such as:

    A-Z
    a-z
    0-9
    +
    /
    =

Therefore, the next step is to decode the data.

## Step 3 – Decode the Base64 Data

Use the `base64` command:

    base64 -d "Logs Data" > decoded

This decodes the Base64 content and writes the resulting binary data into `decoded`.

## Step 4 – Identify the Decoded File

Use:

    file decoded

The output should identify the recovered data as an image file.

If the output identifies it as a JPEG, rename it:

    mv decoded decoded.jpg

If it identifies another image format, use the appropriate extension.

## Step 5 – Inspect the Image

After recovering the image, inspect it using normal forensic tools.

For example:

    strings decoded.jpg | grep -i "pico"

You can also check metadata with:

    exiftool decoded.jpg

The hidden information in the image reveals the flag.

## Step 6 – Get the Flag

The recovered flag is:

    picoCTF{forensics_analysis_is_amazing_5ccc7cb0}

## Why This Works

The challenge hides an image inside a large block of Base64-encoded data.

The process is:

    Encoded log data
            ↓
    Base64 decoding
            ↓
    Binary image data
            ↓
    Recover the image
            ↓
    Analyze the image
            ↓
    Find the hidden flag

Base64 is an encoding method, not encryption. Therefore, once the encoding is identified, the data can be decoded directly.

## Important Concepts

### Base64

Base64 represents binary data using printable characters.

It is commonly used to transfer binary files through text-based systems.

To decode Base64:

    base64 -d input.txt > output

### File Identification

After decoding, use:

    file decoded

This helps determine what type of file was recovered.

### Strings

Readable text embedded in a binary file can be extracted using:

    strings decoded.jpg

Searching for the flag:

    strings decoded.jpg | grep -i "pico"

### ExifTool

Metadata can be inspected with:

    exiftool decoded.jpg

ExifTool is useful when investigating suspicious image files.

## Commands Used

Check the downloaded file:

    ls -lh

Decode Base64:

    base64 -d "Logs Data" > decoded

Identify the decoded file:

    file decoded

Rename if it is a JPEG:

    mv decoded decoded.jpg

Search for the flag:

    strings decoded.jpg | grep -i "pico"

Check metadata:

    exiftool decoded.jpg

## One-Line Solution

    base64 -d "Logs Data" > decoded && file decoded && strings decoded | grep -i "pico"

## Complete Solution

    Download Logs Data
            ↓
    Inspect the large encoded file
            ↓
    Recognize Base64 encoding
            ↓
    Decode using base64 -d
            ↓
    Generate the binary image
            ↓
    Identify the recovered file using file
            ↓
    Analyze the image
            ↓
    Search for hidden information
            ↓
    Find the flag

## Flag

    picoCTF{forensics_analysis_is_amazing_5ccc7cb0}

## Tools Used

- Linux Terminal
- `base64`
- `file`
- `strings`
- `grep`
- `exiftool`

## Key Learning

- Base64 is an encoding scheme, not encryption.
- Large blocks of seemingly random text may contain encoded files.
- `base64 -d` can recover the original binary data.
- `file` helps identify the type of a recovered file.
- `strings` can reveal readable information hidden inside binary files.
- Forensics challenges often require decoding one layer before analyzing the actual file.
