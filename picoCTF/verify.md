# Verify
| **Platform** | picoCTF 2024 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Jeffery John |
| **Topic** | SHA-256 / Checksum / Verification |

## Challenge

The challenge says:

People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.

The challenge provides SSH access:

    Server: rhea.picoctf.net
    Port: 58378
    Username: ctf-player
    Password: f3b61b38

The provided SHA-256 checksum is:

    fba9f49bf22a7188a155768ab0dfdc1f9b86c47976cd0f7c9003af2e20598f7

The challenge also provides a decryption script:

    ./decrypt.sh files/<file>

## Step 1 – Connect to the Server

Connect using SSH:

    ssh -p 58378 ctf-player@rhea.picoctf.net

Password:

    f3b61b38

## Step 2 – List the Files

Run:

    ls

The output contains:

    checksum.txt
    decrypt.sh
    files

Now check the files directory:

    ls files

The encrypted file is:

    87590c24

## Step 3 – Verify the SHA-256 Hash

Calculate the SHA-256 hash of the file:

    sha256sum files/87590c24

The output matches the checksum provided by the challenge:

    fba9f49bf22a7188a155768ab0dfdc1f9b86c47976cd0f7c9003af2e20598f7

This confirms that files/87590c24 is the correct file.

## Step 4 – Find the File Using grep

We can also search all files for the provided hash:

    find files -type f -exec sha256sum {} \; | grep 'fba9f49bf22a7188a155768ab0dfdc1f9b86c47976cd0f7c9003af2e20598f7'

This identifies:

    files/87590c24

## Step 5 – Decrypt the File

After verifying the hash, use the provided decrypt script:

    ./decrypt.sh files/87590c24

The script decrypts the verified file and reveals the flag.

## Flag

    picoCTF{trust_but_verify_87590c24}

## Why This Works

The challenge demonstrates the importance of file integrity verification.

A SHA-256 hash acts like a fingerprint for a file. If the calculated hash matches the hash provided by the challenge, we know that the file is the expected file.

We calculate the hash using:

    sha256sum files/87590c24

The calculated hash matches:

    fba9f49bf22a7188a155768ab0dfdc1f9b86c47976cd0f7c9003af2e20598f7

Therefore, the file is verified and can be decrypted using:

    ./decrypt.sh files/87590c24

## Important Concepts

### SHA-256

SHA-256 is a cryptographic hash function that generates a fixed-size hash from file contents.

Even a small change to a file produces a completely different hash.

### Checksum Verification

The challenge provides a known SHA-256 checksum.

We compare it with the hash generated from the candidate file.

    sha256sum files/87590c24

If both hashes match, the file is verified.

### find

The find command can locate files and execute another command on each file:

    find files -type f -exec sha256sum {} \;

### grep

grep can filter command output and search for the required checksum:

    find files -type f -exec sha256sum {} \; | grep 'HASH'

### Decryption

After identifying and verifying the correct file, the provided script decrypts it:

    ./decrypt.sh files/87590c24

## Commands Used

Connect to the server:

    ssh -p 58378 ctf-player@rhea.picoctf.net

List files:

    ls

List encrypted files:

    ls files

Calculate SHA-256:

    sha256sum files/87590c24

Find the file using its hash:

    find files -type f -exec sha256sum {} \; | grep 'fba9f49bf22a7188a155768ab0dfdc1f9b86c47976cd0f7c9003af2e20598f7'

Decrypt the file:

    ./decrypt.sh files/87590c24

## One-Line Solution

    sha256sum files/87590c24 && ./decrypt.sh files/87590c24

## Complete Solution

    SSH into the server
            ↓
    ls
            ↓
    Find the files directory
            ↓
    ls files
            ↓
    Find 87590c24
            ↓
    Calculate SHA-256 hash
            ↓
    sha256sum files/87590c24
            ↓
    Hash matches the provided checksum
            ↓
    Run decrypt.sh
            ↓
    ./decrypt.sh files/87590c24
            ↓
    Get the flag

## Tools Used

- Linux Terminal
- SSH
- ls
- sha256sum
- find
- grep
- decrypt.sh

## Key Learning

- SHA-256 can be used to verify file integrity.
- A checksum acts as a fingerprint for file contents.
- sha256sum calculates the SHA-256 hash of a file.
- find can be used to locate files and calculate their hashes.
- grep can filter command output.
- Files should be verified before being trusted.
- The challenge demonstrates the security principle: Trust, but verify.
