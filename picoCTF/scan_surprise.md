# Scan Surprise
| **Platform** | picoCTF 2024 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Jeffrey John |
| **Topic** | QR Code / Image Forensics / zbarimg |

## Challenge

The challenge says:

```text
I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?

You can download the challenge files here:
challenge.zip
```

The same files are accessible through SSH:

```text
Server: atlas.picoctf.net
Port: 53404
Username: ctf-player
Password: 83dcefb7
```

The hints are:

```text
QR codes are a way of encoding data. While they're most known for storing URLs, they can store other things too.
```

```text
Mobile phones have included native QR code scanners in their cameras since version 8 (Oreo) and iOS 11
```

```text
If you don't have access to a phone, you can also use zbar-tools to convert an image to text
```

The important clue is the mention of **QR codes** and `zbar-tools`.

## Step 1 – Connect to the Server

Use the SSH command provided by the challenge:

```bash
ssh -p 53404 ctf-player@atlas.picoctf.net
```

Enter the password:

```text
83dcefb7
```

If asked to accept the SSH fingerprint, type:

```text
yes
```

## Step 2 – List the Files

After connecting, run:

```bash
ls
```

The challenge files should be present.

You can also use:

```bash
ls -la
```

to see all files.

## Step 3 – Identify the Image

Use:

```bash
file *
```

This helps identify which file is the QR-code image.

You can also inspect the archive if the challenge was downloaded as a ZIP:

```bash
unzip challenge.zip
```

Then:

```bash
ls -la
```

## Step 4 – Decode the QR Code

The third hint tells us to use `zbar-tools`.

The main command is:

```bash
zbarimg <image-file>
```

For example:

```bash
zbarimg flag.png
```

If the image contains a QR code, `zbarimg` scans it and prints the encoded data.

You may also use:

```bash
zbarimg --raw <image-file>
```

The `--raw` option prints only the decoded QR-code contents.

## Step 5 – Get the Flag

The QR code contains:

```text
picoCTF{p33k_@_b00_3f7cf1ae}
```

Therefore, the flag is:

```text
picoCTF{p33k_@_b00_3f7cf1ae}
```

## Why This Works

The challenge does not store the flag as ordinary text.

Instead, the flag is encoded inside a QR-code image.

The workflow is:

```text
Challenge ZIP
      ↓
Extract the files
      ↓
Find the QR-code image
      ↓
Use zbarimg
      ↓
Decode the QR code
      ↓
Get the flag
```

`zbarimg` is part of the ZBar barcode/QR-code tools and can scan QR codes directly from image files.

## Commands Used

Connect using SSH:

```bash
ssh -p 53404 ctf-player@atlas.picoctf.net
```

List files:

```bash
ls -la
```

Extract the challenge archive if necessary:

```bash
unzip challenge.zip
```

Identify files:

```bash
file *
```

Decode the QR code:

```bash
zbarimg <image-file>
```

Decode without extra formatting:

```bash
zbarimg --raw <image-file>
```

## One-Line Solution

```bash
zbarimg --raw <image-file>
```

## Complete Solution

```text
SSH into the server
        ↓
ssh -p 53404 ctf-player@atlas.picoctf.net
        ↓
Enter password
        ↓
ls -la
        ↓
Find the QR-code image
        ↓
Run zbarimg on the image
        ↓
zbarimg --raw <image-file>
        ↓
Decode the QR code
        ↓
Get the flag
```

## Flag

```text
picoCTF{p33k_@_b00_3f7cf1ae}
```

## Tools Used

- Linux Terminal
- SSH
- `ls`
- `unzip`
- `file`
- `zbarimg`
- QR Code

## Key Learning

- QR codes can store arbitrary text, not only URLs.
- QR codes are useful in CTF challenges for hiding flags.
- `zbarimg` can decode QR codes directly from image files.
- The `--raw` option makes the decoded output easier to read.
- In Forensics challenges, hints often directly point toward the tool needed to extract the hidden information.
- When an image looks suspicious, inspect it with file-analysis and image-decoding tools.

## Final Solution

```text
challenge.zip
      ↓
Extract files
      ↓
Find QR-code image
      ↓
zbarimg --raw <image-file>
      ↓
picoCTF{p33k_@_b00_3f7cf1ae}
```
