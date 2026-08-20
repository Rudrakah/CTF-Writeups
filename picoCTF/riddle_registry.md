# Riddle Registry
| **Platform** | picoMini by CMU-Africa |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Prince Nyonsuthu N. |
| **Topic** | PDF / Metadata / Base64 |

## Challenge

The challenge provides a suspicious PDF containing mostly garbled text.

The description says:

```text
Hi, intrepid investigator! You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered.

Find the PDF file here Hidden Confidential Document and uncover the flag within the metadata.
```

The hints are:

```text
Don't be fooled by the visible text; it's just a decoy!
Look beyond the surface for hidden clues
```

The flag is hidden inside the PDF metadata.

## Step 1 – Download the PDF

Download the provided file.

The downloaded file was:

```text
confidential.pdf.1
```

Check the file type:

```bash
file confidential.pdf.1
```

Output:

```text
confidential.pdf.1: PDF document, version 1.7, 1 pages
```

This confirms that the file is actually a PDF.

## Step 2 – Inspect the Metadata

Use ExifTool:

```bash
exiftool confidential.pdf.1
```

Among the metadata fields, the `Author` field contains encoded data.

The important value is:

```text
cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=
```

This looks like Base64.

## Step 3 – Decode the Base64

Use:

```bash
echo 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=' | base64 -d
```

The decoded output is:

```text
picoCTF{puzzl3d_m3tadata_f0und!_c999e2a4}
```

## Why This Works

The visible text inside the PDF is intentionally misleading.

The challenge tells us to:

```text
Look beyond the surface for hidden clues
```

The useful information is stored in the PDF metadata rather than the visible document contents.

`exiftool` can display PDF metadata such as:

```text
Author
Producer
Creator
Creation Date
PDF Version
```

The `Author` field contains a Base64-encoded string.

After decoding it with:

```bash
base64 -d
```

the flag is revealed.

## Important Concepts

### PDF Metadata

PDF files can contain metadata fields such as:

```text
Author
Producer
Creator
Creation Date
```

ExifTool can extract this information:

```bash
exiftool confidential.pdf.1
```

### Base64

The metadata value:

```text
cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=
```

is Base64 encoded.

Decode it using:

```bash
echo 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=' | base64 -d
```

## Commands Used

Check the file:

```bash
file confidential.pdf.1
```

Extract metadata:

```bash
exiftool confidential.pdf.1
```

Decode the metadata:

```bash
echo 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=' | base64 -d
```

## One-Line Solution

```bash
exiftool confidential.pdf.1 | grep -i Author | awk -F': ' '{print $2}' | base64 -d
```

## Complete Solution

```text
Download confidential.pdf.1
        ↓
Check the file type
        ↓
file confidential.pdf.1
        ↓
Confirm it is a PDF
        ↓
Inspect PDF metadata
        ↓
exiftool confidential.pdf.1
        ↓
Find the Author field
        ↓
Copy the Base64 value
        ↓
Decode using base64 -d
        ↓
Recover the flag
```

## Flag

```text
picoCTF{puzzl3d_m3tadata_f0und!_c999e2a4}
```

## Tools Used

- Linux Terminal
- `file`
- `exiftool`
- `grep`
- `awk`
- `base64`
- PDF metadata analysis

## Key Learning

- Important information may be hidden in file metadata.
- PDF metadata can be inspected using ExifTool.
- Visible file contents are not always the actual source of the flag.
- Base64-encoded strings can often be recognized by their character pattern and trailing `=`.
- `base64 -d` can decode Base64 data.
- In forensic challenges, always inspect metadata in addition to the visible content.

## Final Solution

```text
confidential.pdf.1
        ↓
exiftool confidential.pdf.1
        ↓
Find Author metadata
        ↓
Base64 encoded value
        ↓
base64 -d
        ↓
picoCTF{puzzl3d_m3tadata_f0und!_c999e2a4}
```
