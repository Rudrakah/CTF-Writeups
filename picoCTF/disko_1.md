# DISKO 1
| **Platform** | picoCTF |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Darkraicg492 |
| **Topic** | Disk Image / Strings / Forensics |

## Challenge

The challenge asks:

Can you find the flag in this disk image?

Download the disk image provided by the challenge.

The hint says:

Maybe Strings could help? If only there was a way to do that?

This hint directly suggests using the `strings` command to search the disk image for readable text.

## Step 1 – Download the Disk Image

Download the disk image from the challenge.

Check the downloaded file:

    ls -lh

Identify the file type:

    file disko-1.dd

The file is a disk image.

## Step 2 – Use Strings

The hint directly points toward the `strings` command.

Run:

    strings disko-1.dd

This extracts readable ASCII strings from the disk image.

Since the disk image may contain a large amount of data, searching directly for `picoCTF` is easier.

Run:

    strings disko-1.dd | grep -i "picoCTF"

## Step 3 – Find the Flag

The command reveals the hidden flag:

    picoCTF{1t5_ju5t_4_5tr1n9_e3408eef}

## Why This Works

A disk image contains raw filesystem and file data.

Even if the flag is stored somewhere inside the image, readable text can often be extracted using the `strings` command.

The challenge hint specifically suggests this technique.

The process is:

    Disk Image
        ↓
    strings
        ↓
    Search for picoCTF
        ↓
    Find hidden flag

## Important Concepts

### Disk Image

A disk image is a file containing a representation of a storage device or filesystem.

It may contain:

- Files
- Deleted files
- Metadata
- Filesystem structures
- Raw data
- Hidden information

### Strings

The `strings` command extracts printable character sequences from binary files.

For example:

    strings file.bin

can reveal readable text hidden inside a binary file.

### Grep

`grep` can filter the output and search for a specific word.

For example:

    strings disko-1.dd | grep -i "picoCTF"

The `-i` option makes the search case-insensitive.

## Commands Used

Check the files:

    ls -lh

Identify the disk image:

    file disko-1.dd

Extract readable strings:

    strings disko-1.dd

Search directly for the flag:

    strings disko-1.dd | grep -i "picoCTF"

## One-Line Solution

    strings disko-1.dd | grep -i "picoCTF"

## Complete Solution

    Download the disk image
            ↓
    Check the file
            ↓
    file disko-1.dd
            ↓
    Use strings
            ↓
    strings disko-1.dd
            ↓
    Search for picoCTF
            ↓
    strings disko-1.dd | grep -i "picoCTF"
            ↓
    Find the flag

## Flag

    picoCTF{1t5_ju5t_4_5tr1n9_e3408eef}

## Tools Used

- Linux Terminal
- `file`
- `strings`
- `grep`
- Disk Image

## Key Learning

- Disk images can contain hidden readable information.
- `strings` is useful for quickly extracting printable text from binary files.
- `grep` can filter large amounts of command output.
- Always read the challenge hints because they often directly indicate the intended tool.
- For simple forensic challenges, start with `file`, `strings`, and `grep`.

## Final Solution

    disko-1.dd
         ↓
    strings disko-1.dd
         ↓
    grep -i "picoCTF"
         ↓
    picoCTF{1t5_ju5t_4_5tr1n9_e3408eef}
