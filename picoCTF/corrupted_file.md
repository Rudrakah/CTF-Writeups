# Corrupted File
| **Platform** | picoMini by CMU-Africa |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | JPEG / File Header / Hex Editing |

## Challenge

The challenge says:

This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?

The hints are:

- Try checking the file's header.
- JPEG
- Tools like `xxd` or `hexump` can help you inspect and edit file bytes.

The challenge provides a corrupted JPEG file.

## Step 1 – Download the File

Download the provided file.

Check the file type:

    file file

The output initially shows:

    file: data

This means the system does not recognize it as a valid JPEG.

## Step 2 – Inspect the File Header

Use `xxd` to inspect the first bytes:

    xxd -l 16 file

The beginning of the file is:

    5c 78 ff e0 00 10 4a 46 49 46 00 01 01 00 00 01

A valid JPEG file should begin with the JPEG magic bytes:

    ff d8 ff e0

The first bytes are corrupted:

    5c 78

They should be:

    ff d8

Therefore, the first two bytes need to be repaired.

## Step 3 – Repair the Header

Use Python to replace the first two bytes:

    python3 -c "p='file'; b=bytearray(open(p,'rb').read()); b[:2]=b'\xff\xd8'; open('fixed.jpg','wb').write(b)"

This creates:

    fixed.jpg

## Step 4 – Verify the Repaired File

Run:

    file fixed.jpg

The output now identifies it as a JPEG image:

    fixed.jpg: JPEG image data, JFIF standard 1.01, aspect ratio 1x1, segment length 16, baseline, precision 8, 800x500, components 3

This confirms that the JPEG header has been successfully repaired.

## Step 5 – Find the Flag

After repairing the image, search its contents for readable text:

    strings fixed.jpg | grep -i "picoCTF"

The hidden flag is:

    picoCTF{r3st0r1ng_th3_by73s_1512b52a}

## Why This Works

Files usually contain specific magic bytes at the beginning that identify their format.

A JPEG normally begins with:

    FF D8 FF

In this challenge, the first two bytes were corrupted.

Original corrupted header:

    5c 78 ff e0

Correct JPEG header:

    ff d8 ff e0

Replacing the corrupted bytes restores the JPEG file structure.

The process is:

    Corrupted file
          ↓
    Inspect header
          ↓
    Find incorrect bytes
          ↓
    Replace 5c 78 with ff d8
          ↓
    Valid JPEG
          ↓
    Search strings
          ↓
    Find flag

## Important Concepts

### File Magic Bytes

Magic bytes are special bytes at the beginning of a file that identify its format.

For JPEG:

    FF D8 FF

For PNG:

    89 50 4E 47

For ZIP:

    50 4B 03 04

### Hexdump / xxd

`xxd` allows us to inspect raw bytes:

    xxd -l 16 file

This is useful when investigating corrupted files.

### File Repair

A corrupted file header can sometimes be repaired by replacing incorrect bytes with the correct magic bytes.

## Commands Used

Check the file:

    file file

Inspect the header:

    xxd -l 16 file

Repair the first two bytes:

    python3 -c "p='file'; b=bytearray(open(p,'rb').read()); b[:2]=b'\xff\xd8'; open('fixed.jpg','wb').write(b)"

Verify the repaired file:

    file fixed.jpg

Search for the flag:

    strings fixed.jpg | grep -i "picoCTF"

## One-Line Solution

    python3 -c "p='file'; b=bytearray(open(p,'rb').read()); b[:2]=b'\xff\xd8'; open('fixed.jpg','wb').write(b)" && strings fixed.jpg | grep -i "picoCTF"

## Complete Solution

    Download the corrupted file
            ↓
    file file
            ↓
    xxd -l 16 file
            ↓
    Identify corrupted JPEG header
            ↓
    Replace 5c 78 with ff d8
            ↓
    Create fixed.jpg
            ↓
    file fixed.jpg
            ↓
    Confirm JPEG format
            ↓
    strings fixed.jpg | grep -i "picoCTF"
            ↓
    Get the flag

## Flag

    picoCTF{r3st0r1ng_th3_by73s_1512b52a}

## Tools Used

- Linux Terminal
- `file`
- `xxd`
- Python
- `strings`
- `grep`
- Hexadecimal / Magic Bytes

## Key Learning

- File headers contain important information about file types.
- JPEG files start with specific magic bytes.
- `xxd` can be used to inspect raw file bytes.
- A few corrupted bytes can make a valid file appear as generic data.
- Python can be used to modify individual bytes in a binary file.
- After repairing a file, `file` can verify its format.
- `strings` can reveal hidden readable information inside binary files.

## Final Solution

    file
      ↓
    xxd -l 16 file
      ↓
    Find corrupted bytes: 5c 78
      ↓
    Replace with JPEG bytes: ff d8
      ↓
    fixed.jpg
      ↓
    strings fixed.jpg | grep -i "picoCTF"
      ↓
    picoCTF{r3st0r1ng_th3_by73s_1512b52a}
