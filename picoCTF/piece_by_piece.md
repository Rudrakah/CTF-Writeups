# Piece by Piece
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | Linux / ZIP / File Splitting |

## Challenge
The challenge says:

```text
After logging in, you will find multiple file parts in your home directory. These parts need to be combined and extracted to reveal the flag.
```

SSH details:

```text
Server: dolphin-cove.picoctf.net
Port: 63571
Username: ctf-player
Password: 1db87a14
```

The instructions explain:

```text
- The flag is split into multiple parts as a zipped file.
- Use Linux commands to combine the parts into one file.
- The zip file is password protected.
- Use the "supersecret" password to extract the zip file.
- After unzipping, check the extracted text file for the flag.
```

## Step 1 – Connect to the Server

```bash
ssh -p 63571 ctf-player@dolphin-cove.picoctf.net
```

Password:

```text
1db87a14
```

## Step 2 – Check the Files

```bash
ls -la
```

We find:

```text
instructions.txt
part_aa
part_ab
part_ac
part_ad
part_ae
```

Read the instructions:

```bash
cat instructions.txt
```

The important information is that the files are pieces of a ZIP archive and the password is:

```text
supersecret
```

## Step 3 – Combine the Parts

The pieces must be combined in the correct order:

```bash
cat part_aa part_ab part_ac part_ad part_ae > flag.zip
```

This creates:

```text
flag.zip
```

The order is important because each file contains a consecutive part of the original ZIP archive.

## Step 4 – Verify the ZIP

Check the file type:

```bash
file flag.zip
```

It should identify the file as a ZIP archive.

## Step 5 – Extract the ZIP

The archive is password protected.

Use:

```bash
unzip -P supersecret flag.zip
```

This extracts:

```text
flag.txt
```

## Step 6 – Read the Flag

```bash
cat flag.txt
```

Output:

```text
picoCTF{zip_and_spl1t_f1l3s_4r3_fun_574adc66}
```

## Why This Works
The challenge splits one ZIP archive into multiple files:

```text
part_aa
part_ab
part_ac
part_ad
part_ae
```

Using `cat` combines them back into the original archive:

```bash
cat part_aa part_ab part_ac part_ad part_ae > flag.zip
```

The reconstructed ZIP is password protected, so we use:

```bash
unzip -P supersecret flag.zip
```

After extraction, the flag is inside:

```text
flag.txt
```

## Important Concepts

### File Concatenation

`cat` can combine multiple files:

```bash
cat file1 file2 file3 > combined
```

### Output Redirection

The `>` operator writes command output into a file:

```bash
command > output
```

### ZIP Extraction

A password-protected ZIP can be extracted with:

```bash
unzip -P password archive.zip
```

### File Identification

The `file` command identifies the type of a file:

```bash
file flag.zip
```

## Commands Used

```bash
ssh -p 63571 ctf-player@dolphin-cove.picoctf.net
```

```bash
ls -la
```

```bash
cat instructions.txt
```

```bash
cat part_aa part_ab part_ac part_ad part_ae > flag.zip
```

```bash
file flag.zip
```

```bash
unzip -P supersecret flag.zip
```

```bash
cat flag.txt
```

## Complete Solution

```text
SSH into the server
        ↓
ls -la
        ↓
Find part_aa through part_ae
        ↓
cat instructions.txt
        ↓
Find the password: supersecret
        ↓
Combine the parts
        ↓
cat part_aa part_ab part_ac part_ad part_ae > flag.zip
        ↓
Verify flag.zip
        ↓
file flag.zip
        ↓
Extract the ZIP
        ↓
unzip -P supersecret flag.zip
        ↓
Find flag.txt
        ↓
cat flag.txt
        ↓
Get the flag
```

## One-Line Solution

```bash
cat part_aa part_ab part_ac part_ad part_ae > flag.zip && unzip -P supersecret flag.zip && cat flag.txt
```

## Flag

```text
picoCTF{zip_and_spl1t_f1l3s_4r3_fun_574adc66}
```

## Tools Used

- Linux Terminal
- SSH
- `ls`
- `cat`
- `file`
- `unzip`
- ZIP archives
- Output redirection

## Key Learning

- Multiple file parts can be combined using `cat`.
- The order of split files matters.
- `>` can redirect output into a new file.
- `file` can verify an archive's file type.
- `unzip` can extract password-protected ZIP archives.
- Challenge instructions often contain important clues needed to solve the challenge.
