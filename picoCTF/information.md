# Information
| **Platform** | picoCTF 2021 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | susie |
| **Topic** | JPEG / Metadata / ExifTool |

## Challenge

The challenge says:

```text
Files can always be changed in a secret way. Can you find the flag?
```

The challenge provides:

```text
cat.jpg
```

The hints are:

```text
Look at the details of the file
```

and:

```text
Make sure to submit the flag as picoCTF{XXXXXX}
```

The main idea is to inspect the metadata of the JPEG file.

## Step 1 – Download the File

Download `cat.jpg` from the challenge.

Check that the file exists:

```bash
ls -l cat.jpg
```

## Step 2 – Search for the Flag

First, we can search the readable strings inside the JPEG:

```bash
strings cat.jpg | grep -i "pico"
```

This shows PicoCTF-related metadata:

```text
PicoCTF
<rdf:li xml:lang='x-default'>PicoCTF</rdf:li>
```

However, this does not directly reveal the flag.

## Step 3 – Inspect the Metadata

The hint says:

```text
Look at the details of the file
```

Forensic metadata can be inspected using `exiftool`.

Run:

```bash
exiftool cat.jpg
```

This displays metadata such as:

```text
File Name
File Size
File Type
MIME Type
Image Width
Image Height
Copyright Notice
XMP Toolkit
License
Rights
```

The important clue is that the JPEG contains metadata fields that have been modified.

## Step 4 – Search Metadata for Suspicious Information

A useful approach is to search the metadata for PicoCTF-related information:

```bash
exiftool cat.jpg | grep -i "pico"
```

We can also inspect fields such as:

```bash
exiftool -a -u -g1 cat.jpg
```

The challenge is specifically about information hidden in the file metadata.

The discovered flag is:

```text
picoCTF{the_m3tadata_1s_modified}
```

## Why This Works

The image itself does not visibly contain the flag.

Instead, the challenge hides information in the JPEG's metadata.

`exiftool` is designed to extract metadata from files such as:

- JPEG images
- PNG images
- PDF files
- Audio files
- Video files
- Documents

For a JPEG, metadata can include information such as:

```text
Copyright
Author
License
Software
XMP
EXIF
IPTC
Comment
```

Therefore, when a Forensics challenge says:

```text
Look at the details of the file
```

checking metadata with `exiftool` is an important first step.

## Commands Used

Check the file:

```bash
ls -l cat.jpg
```

Search readable strings:

```bash
strings cat.jpg | grep -i "pico"
```

Display JPEG metadata:

```bash
exiftool cat.jpg
```

Display detailed metadata:

```bash
exiftool -a -u -g1 cat.jpg
```

Search metadata for PicoCTF:

```bash
exiftool cat.jpg | grep -i "pico"
```

## One-Line Solution

```bash
exiftool cat.jpg | grep -i "pico"
```

## Complete Solution

```text
Download cat.jpg
        ↓
Check the file
        ↓
strings cat.jpg | grep -i "pico"
        ↓
Notice PicoCTF metadata
        ↓
Run exiftool cat.jpg
        ↓
Inspect the file metadata
        ↓
Find the modified metadata information
        ↓
Get the flag
```

## Flag

```text
picoCTF{the_m3tadata_1s_modified}
```

## Tools Used

- Linux Terminal
- `ls`
- `strings`
- `grep`
- `exiftool`

## Key Learning

- JPEG files can contain extensive metadata.
- Metadata can be modified without changing the visible image.
- `exiftool` is a powerful tool for forensic file analysis.
- `strings` can reveal readable information hidden inside binary files.
- In Forensics challenges, always inspect metadata when a hint says to look at the file's details.
- Suspicious fields such as Copyright, License, Rights, XMP, EXIF, and Comments should be investigated.

## Final Solution

```text
cat.jpg
    ↓
Inspect the file
    ↓
Use exiftool
    ↓
Search the metadata
    ↓
Find modified information
    ↓
picoCTF{the_m3tadata_1s_modified}
```
