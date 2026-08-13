# Password Profiler
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | OSINT / Password Cracking / SHA-1 |

## Challenge

The challenge says:

```text
We intercepted a suspicious file from a system, but instead of the password itself, it only contains its SHA-1 hash. Using OSINT techniques, you are provided with personal details about the target. Your task is to leverage this information to generate a custom password list and recover the original password by matching its hash.
```

The challenge provides three files:

```text
userinfo
hash
check_password
```

Their purposes are:

```text
userinfo       → Contains personal details about the target
hash           → Contains the SHA-1 hash of the password
check_password → Script used to test passwords against the hash
```

The hint says:

```text
CUPP is a Python tool for generating custom wordlists from personal data.
```

The main idea is to use the personal information from `userinfo` to generate possible passwords, then compare their SHA-1 hashes with the provided hash.

## Step 1 – Download the Files

Download all three files from the challenge page:

```text
userinfo
hash
check_password
```

Check that they are present:

```bash
ls
```

We should see:

```text
userinfo
hash
check_password
```

## Step 2 – Read the Personal Information

First inspect the `userinfo` file:

```bash
cat userinfo
```

This file contains personal information about the target.

The information can include details such as:

```text
Name
Username
Birth information
Family information
Other personal details
```

These details are useful because people often create passwords using information that is easy for them to remember.

## Step 3 – Read the SHA-1 Hash

Inspect the hash:

```bash
cat hash
```

The file contains the SHA-1 hash of the original password.

The goal is to find a candidate password whose SHA-1 hash matches this value.

Conceptually:

```text
Candidate password
        ↓
SHA-1
        ↓
Generated hash
        ↓
Compare with hash from hash file
```

If the hashes match, we have recovered the password.

## Step 4 – Generate a Custom Wordlist

The challenge hint points directly to **CUPP**.

CUPP stands for:

```text
Common User Passwords Profiler
```

It can generate custom password lists based on information about a target.

The basic command is:

```bash
cupp -i
```

CUPP will ask for information about the target.

Use the information obtained from:

```text
userinfo
```

to answer the questions.

CUPP will then generate a custom wordlist containing many possible passwords.

The generated wordlist is typically stored as:

```text
<target>.txt
```

## Step 5 – Check the Password Script

Inspect the provided script:

```bash
cat check_password
```

The script is designed to test password candidates against the SHA-1 hash.

The important concept is that each candidate password is hashed and compared against the supplied hash.

The script helps automate the checking process.

## Step 6 – Use the Generated Wordlist

After generating the custom wordlist, identify the generated file:

```bash
ls
```

Then use the provided `check_password` script with the generated password list according to its usage information.

If necessary, inspect the script's usage:

```bash
python3 check_password
```

or:

```bash
python3 check_password --help
```

The exact arguments depend on the supplied challenge script.

The goal is:

```text
userinfo
    ↓
CUPP
    ↓
Custom password wordlist
    ↓
check_password
    ↓
SHA-1 comparison
    ↓
Correct password
```

## Step 7 – Recover the Password

The correct password is the candidate whose SHA-1 hash matches the value stored in:

```text
hash
```

Once the matching candidate is found, the challenge can be completed.

The important point is that we do not need to reverse SHA-1 itself.

Instead, we perform a targeted dictionary attack using passwords generated from the target's personal information.

## Why This Works

The challenge is based on a common password-security weakness.

Passwords are often created using information that is personally meaningful to the user.

For example:

```text
Name
Nickname
Birth year
Favorite number
Family member
Pet name
```

CUPP combines such information to create likely password candidates.

The SHA-1 hash cannot simply be reversed mathematically.

Instead, we try possible passwords:

```text
password candidate
        ↓
SHA-1(candidate)
        ↓
Compare with target hash
```

When:

```text
SHA-1(candidate) == target_hash
```

the candidate is the original password.

## Important Concepts

### 1. OSINT

OSINT stands for:

```text
Open Source Intelligence
```

It involves collecting useful information about a target from available information sources.

In this challenge, the supplied `userinfo` file acts as our source of personal information.

### 2. Custom Wordlists

A custom wordlist is generated specifically for a target.

Instead of trying millions of completely random passwords, we create candidates based on known information.

This makes password guessing much more efficient.

### 3. CUPP

CUPP is a tool for generating custom password wordlists.

The interactive mode can be started with:

```bash
cupp -i
```

### 4. SHA-1

SHA-1 is a cryptographic hash function.

For a password:

```text
password
```

SHA-1 produces a fixed-length hexadecimal hash.

For example:

```text
password
    ↓
SHA-1
    ↓
hash
```

The important property is that we normally cannot directly recover the original password from the hash.

Instead, we test candidate passwords.

### 5. Dictionary Attack

A dictionary attack tests a list of possible passwords.

In this challenge:

```text
CUPP-generated wordlist
        ↓
Password candidates
        ↓
SHA-1 each candidate
        ↓
Compare with supplied hash
        ↓
Matching password
```

## Commands Used

List the challenge files:

```bash
ls
```

Read the personal information:

```bash
cat userinfo
```

Read the SHA-1 hash:

```bash
cat hash
```

Inspect the password-checking script:

```bash
cat check_password
```

Run CUPP interactively:

```bash
cupp -i
```

Check generated files:

```bash
ls
```

Run the supplied password-checking script according to its usage:

```bash
python3 check_password
```

## Complete Solution

```text
Download the challenge files
        ↓
Inspect userinfo
        ↓
Collect the target's personal information
        ↓
Inspect hash
        ↓
Identify the SHA-1 hash
        ↓
Inspect check_password
        ↓
Use CUPP
        ↓
cupp -i
        ↓
Enter information from userinfo
        ↓
Generate a custom password wordlist
        ↓
Test the candidates using check_password
        ↓
SHA-1 candidate passwords
        ↓
Compare with the supplied hash
        ↓
Find the matching password
        ↓
Submit the flag
```

## Key Learning

- OSINT information can be used to create targeted password lists.
- CUPP can generate custom wordlists from personal information.
- SHA-1 is a hash function and is not normally reversed directly.
- Dictionary attacks work by hashing candidate passwords and comparing the results.
- Custom wordlists can be much more effective than generic password lists.
- Personal information should not be used directly in passwords because it can make password guessing easier.
- Modern systems should use strong password hashing algorithms designed for password storage rather than SHA-1.

## Flag

```text
picoCTF{Aj_15901990}
```

## Tools Used

- Linux Terminal
- Python
- CUPP
- SHA-1
- Custom Wordlists
- OSINT
- Dictionary Attack

## Final Answer

```text
picoCTF{Aj_15901990}
```
