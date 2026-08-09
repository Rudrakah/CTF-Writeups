# Nothing Up My Sleeve
| **Platform** | picoCTF 2020 Mini-Competition |
| **Category** | General Skills |
| **Difficulty** | Medium |

## Challenge
The challenge says:
```text
Let's check that your internet connection is working. This flag is 'in-the-clear', I promise!
```
The challenge provides a file named:
```text
flag.txt
```
The hint says:
```text
If all you had access to was a shell, you could use wget to download the file at the URL above!
```
The phrase **"in-the-clear"** is the main clue. It means the flag is not encrypted, encoded, or hidden. We simply need to download and read the file.
## Step 1 – Download the File
If using the picoCTF Webshell, we can download the provided file using `wget`:
```bash
wget <flag.txt-download-url>
```
Then check the downloaded files:
```bash
ls
```
We should see:
```text
flag.txt
```
## Step 2 – Read the File
Use the `cat` command:
```bash
cat flag.txt
```
The contents of the file directly reveal the flag.
No decoding, encryption, hashing, or exploitation is required.
## Step 3 – Identify the Flag
The `flag.txt` file contains:
```text
picoCTF{c0ng4ts_on_y0ur_s4n1ty}
```
Since the flag is already in the correct picoCTF format, we can submit it directly.
## Why This Works
The challenge description explicitly says:
```text
This flag is 'in-the-clear'
```
"In the clear" means the information is stored in its original readable form rather than being encrypted or encoded.
Therefore, the solution is simply:
```text
Download flag.txt
        ↓
Read flag.txt
        ↓
Flag is directly visible
        ↓
Submit the flag
```

## Commands Used
Download the file:
```bash
wget <flag.txt-download-url>
```
List files:
```bash
ls
```
Read the flag:
```bash
cat flag.txt
```

## Flag
```text
picoCTF{c0ng4ts_on_y0ur_s4n1ty}
```

## Tools Used
- Linux Terminal
- `wget`
- `ls`
- `cat`

## Key Learning
- Files provided by CTF challenges should always be inspected first.
- `wget` can download files directly from a URL through the terminal.
- `cat` is useful for quickly displaying text files.
- "In the clear" means the data is directly readable.
- Not every CTF requires encoding, encryption, or exploitation.

## Final Solution
The entire solution can be summarized as:
```text
Open Nothing Up My Sleeve
        ↓
Download flag.txt
        ↓
ls
        ↓
cat flag.txt
        ↓
Flag is directly visible
        ↓
picoCTF{c0ng4ts_on_y0ur_s4n1ty}
        ↓
Submit
        ↓
Correct flag!
```
