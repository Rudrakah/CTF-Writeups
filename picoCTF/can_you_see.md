# CanYouSee
| **Platform** | picoCTF 2024 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Mubarak Mikail |
| **Topic** | Image Metadata / Base64 / ExifTool |

## Challenge
The challenge says:

```text
How about some hide and seek?

Download this file here.
```

The hints are:

```text
How can you view the information about the picture?
```

and:

```text
If something isn't in the expected form, maybe it deserves attention?
```

The main idea is to inspect the metadata of the downloaded image.

## Step 1 – Download the File

Download the challenge image.

Check the downloaded file:

```bash
ls -l
```

## Step 2 – Identify the File

Use:

```bash
file <filename>
```

This confirms the file type.

## Step 3 – Inspect the Image Metadata

The first hint says to look at the information about the picture.

Use `exiftool`:

```bash
exiftool <filename>
```

Look carefully through the metadata for unusual or suspicious fields.

One of the metadata fields contains an encoded string.

## Step 4 – Find the Suspicious Data

You can search the metadata for unusual long strings:

```bash
exiftool <filename> | less
```

The suspicious value is encoded using Base64.

Base64 strings commonly contain characters such as:

```text
A-Z
a-z
0-9
+
/
=
```

The challenge hint says that something not being in the expected form deserves attention, so the unusual metadata value is the important clue.

## Step 5 – Decode the Base64

Copy the suspicious Base64 value and decode it using:

```bash
echo '<base64-string>' | base64 -d
```

The decoded output gives the flag:

```text
picoCTF{ME74D47A_H1DD3N_a6df8db8}
```

## Why This Works

The flag is not simply visible in the image.

Instead, information is hidden inside the image metadata.

`exiftool` allows us to inspect metadata such as:

```text
File Name
File Type
Image Size
Software
Comment
Description
XMP
EXIF
```

The suspicious metadata value is Base64 encoded.

Therefore, the solving process is:

```text
Image
   ↓
Metadata
   ↓
Find unusual Base64 value
   ↓
Base64 decode
   ↓
Flag
```

## Commands Used

List files:

```bash
ls -l
```

Identify the image:

```bash
file <filename>
```

Inspect metadata:

```bash
exiftool <filename>
```

View metadata more easily:

```bash
exiftool <filename> | less
```

Decode the suspicious Base64 value:

```bash
echo '<base64-string>' | base64 -d
```

## One-Line Solution

```bash
exiftool <filename> | grep -E '[A-Za-z0-9+/]{20,}={0,2}'
```

Then decode the suspicious Base64 value with:

```bash
echo '<base64-string>' | base64 -d
```

## Complete Solution

```text
Download the image
        ↓
Run file <filename>
        ↓
Inspect metadata
        ↓
exiftool <filename>
        ↓
Find the unusual Base64 string
        ↓
Copy the encoded value
        ↓
echo '<base64-string>' | base64 -d
        ↓
Get the hidden flag
```

## Flag

```text
picoCTF{ME74D47A_H1DD3N_a6df8db8}
```

## Tools Used

- Linux Terminal
- `file`
- `exiftool`
- `grep`
- `less`
- `base64`

## Key Learning

- Images can contain hidden information inside metadata.
- `exiftool` is an important tool for image forensics.
- Suspicious metadata should always be investigated.
- Base64 is an encoding, not encryption.
- `base64 -d` can decode Base64 data from the terminal.
- In image-forensics challenges, always inspect metadata before assuming the flag is hidden in the visible image.

## Final Solution

```text
Image
   ↓
exiftool
   ↓
Inspect metadata
   ↓
Find suspicious Base64
   ↓
base64 -d
   ↓
picoCTF{ME74D47A_H1DD3N_a6df8db8}
```
