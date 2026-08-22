# hashcrack
| **Platform** | picoCTF 2025 |
| **Category** | Cryptography |
| **Difficulty** | Easy |
| **Author** | Nana Ama Atombo-Sackey |
| **Topic** | Hashing / Hash Cracking / Weak Passwords |

## Challenge

A company stored a secret message on a server which got breached because the admin used weakly hashed passwords.

The goal is to access the server, identify the password hashes, crack them, and use the recovered password to obtain the secret flag.

Server:

    nc verbal-sleep.picoctf.net 63779

## Step 1 – Connect to the Server

Connect using:

    nc verbal-sleep.picoctf.net 63779

The server provides information containing password hashes.

The important part is to identify the hashing algorithm from the hash length and format.

## Step 2 – Identify the Hash Type

Common hash lengths include:

    MD5    = 32 hexadecimal characters
    SHA-1  = 40 hexadecimal characters
    SHA-256 = 64 hexadecimal characters

The challenge hint tells us to carefully inspect the length and structure of the hashes.

Once the algorithm is identified, the hash can be cracked using a password-cracking tool or an online hash database.

## Step 3 – Crack the Weak Hashes

Because the challenge specifically says the passwords are weakly hashed, dictionary-based cracking is effective.

Useful tools include:

    hashcat
    john
    rockyou.txt

For example, with Hashcat:

    hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt

The `-m 0` mode is used for MD5 hashes.

If the hash is SHA-1, use:

    hashcat -m 100 hashes.txt /usr/share/wordlists/rockyou.txt

The recovered password can then be used to authenticate to the server.

## Step 4 – Access the Secret

After cracking the administrator password, use the recovered credentials as requested by the server.

The server eventually reveals the secret message/flag.

## Why This Works

The challenge demonstrates why weak password hashing is dangerous.

If a password is hashed with a weak algorithm and the password itself is simple, attackers can try large dictionaries of common passwords until the generated hash matches the stolen hash.

The important process is:

    Obtain hash
        ↓
    Identify hash algorithm
        ↓
    Run dictionary attack
        ↓
    Recover weak password
        ↓
    Authenticate to server
        ↓
    Access secret
        ↓
    Get flag

## Useful Hash-Cracking Commands

### Hashcat

    hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt

### SHA-1 with Hashcat

    hashcat -m 100 hashes.txt /usr/share/wordlists/rockyou.txt

### John the Ripper

    john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

Show recovered passwords:

    john --show hashes.txt

## One-Line Solution

    nc verbal-sleep.picoctf.net 63779

Then identify the hashes, crack the weak password hashes with a dictionary attack, authenticate using the recovered password, and retrieve the secret.

## Complete Solution

    Connect to the server
            ↓
    nc verbal-sleep.picoctf.net 63779
            ↓
    Inspect the supplied hashes
            ↓
    Identify the hash algorithm from length/format
            ↓
    Use Hashcat/John with rockyou.txt
            ↓
    Recover the weak password
            ↓
    Authenticate to the server
            ↓
    Retrieve the secret
            ↓
    Flag obtained

## Flag

    picoCTF{UseStr0nG_h@shEs_&Pa$$w0rds!_ccc21957}

## Tools Used

- Linux Terminal
- `nc`
- Hashcat
- John the Ripper
- `rockyou.txt`
- Hash identification

## Key Learning

- Hashes are one-way representations of data, but weak passwords can still be cracked through guessing.
- Hash length and format can help identify the hashing algorithm.
- Weak passwords make dictionary attacks highly effective.
- Password hashes should be protected even if the original passwords are not directly stored.
- Modern password storage should use strong password-hashing algorithms such as Argon2, bcrypt, or scrypt with appropriate salts.
- Tools such as Hashcat and John the Ripper are useful for testing password strength.

## Final Solution

    nc verbal-sleep.picoctf.net 63779
            ↓
    Identify the hashes
            ↓
    Crack the weak hashes
            ↓
    Recover the password
            ↓
    Access the secret
            ↓
    picoCTF{UseStr0nG_h@shEs_&Pa$$w0rds!_ccc21957}
