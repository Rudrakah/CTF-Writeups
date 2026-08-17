# Hidden in Plainsight
| **Platform** | picoMini by CMU-Africa |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | JPEG / Metadata / Steganography |

## Challenge

The challenge says:

You're given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.

Download the JPG image from the challenge.

The hint says:

Download the jpg image and read its metadata.

## Step 1 – Download the Image

Download the provided image.

Check the file:

    ls -lh

Identify the file type:

    file img.jpg

## Step 2 – Read the Metadata

Use `exiftool` to inspect the image metadata:

    exiftool img.jpg

The metadata contains a suspicious `Comment` field.

The value in the Comment field is Base64 encoded.

## Step 3 – Decode the Metadata

Copy the value from the `Comment` field and decode it:

    echo 'BASE64_VALUE' | base64 -d

The decoded information provides a clue related to steganography and reveals the password needed for extraction.

## Step 4 – Identify Steghide

Check whether `steghide` is available:

    which steghide

The challenge uses `steghide` to hide data inside the JPG.

The password obtained from the metadata is:

    pAZzword

## Step 5 – Extract the Hidden File

Run:

    steghide extract -sf img.jpg -p 'pAZzword'

If `flag.txt` already exists, choose `y` when asked whether to overwrite it.

The hidden file is:

    flag.txt

## Step 6 – Read the Flag

Run:

    cat flag.txt

The output is:

    picoCTF{h1dd3n_1n_1m4g3_656e4d79}

## Why This Works

The flag is hidden inside the JPG using multiple layers.

The solution path is:

    JPG image
        ↓
    Exif metadata
        ↓
    Suspicious Comment field
        ↓
    Base64 decode
        ↓
    Obtain steganography clue/password
        ↓
    Steghide extraction
        ↓
    flag.txt
        ↓
    Flag

## Important Concepts

### Exif Metadata

Images can contain metadata such as:

- Camera information
- Software information
- Date/time
- Comments
- Author information
- GPS information

`exiftool` can display this information:

    exiftool img.jpg

### Base64

The suspicious metadata value is Base64 encoded.

It can be decoded using:

    echo 'BASE64_VALUE' | base64 -d

### Steghide

`steghide` is a steganography tool that can hide data inside image files.

To extract hidden data:

    steghide extract -sf img.jpg -p 'pAZzword'

## Commands Used

Check the file:

    file img.jpg

Read metadata:

    exiftool img.jpg

Decode the suspicious Base64 value:

    echo 'BASE64_VALUE' | base64 -d

Check steghide:

    which steghide

Extract the hidden file:

    steghide extract -sf img.jpg -p 'pAZzword'

Read the flag:

    cat flag.txt

## One-Line Solution

    exiftool img.jpg
    echo 'BASE64_VALUE' | base64 -d
    steghide extract -sf img.jpg -p 'pAZzword'
    cat flag.txt

## Complete Solution

    Download img.jpg
            ↓
    exiftool img.jpg
            ↓
    Find suspicious Comment metadata
            ↓
    Decode Comment using Base64
            ↓
    Find steghide clue/password
            ↓
    Run steghide extract
            ↓
    Extract flag.txt
            ↓
    cat flag.txt
            ↓
    Get the flag

## Flag

    picoCTF{h1dd3n_1n_1m4g3_656e4d79}

## Tools Used

- Linux Terminal
- `file`
- `exiftool`
- `base64`
- `steghide`
- `cat`

## Key Learning

- Always inspect image metadata during forensic investigations.
- `exiftool` is useful for finding suspicious metadata.
- Metadata can contain encoded clues.
- Base64 data can be decoded using `base64 -d`.
- Steganography can hide files inside images.
- `steghide` can extract hidden data from supported image formats.
- A file can contain multiple layers of hidden information.

## Final Solution

    img.jpg
       ↓
    exiftool img.jpg
       ↓
    Suspicious Comment
       ↓
    Base64 Decode
       ↓
    Steghide + Password
       ↓
    flag.txt
       ↓
    cat flag.txt
       ↓
    picoCTF{h1dd3n_1n_1m4g3_656e4d79}
