# Shared Secrets
| **Platform** | picoCTF 2026 |
| **Category** | Cryptography |
| **Difficulty** | Easy |
| **Author** | Yahaya Meddy |
| **Topic** | Diffie-Hellman / Shared Secret / XOR |

## Challenge

A message was encrypted using a shared secret, but one side of the exchange leaked something. The goal is to recover the shared secret and decrypt the message.

The challenge provides a message file and the source code used for the encryption.

## Step 1 – Inspect the Source Code

Open `encryption.py` and inspect how the Diffie-Hellman values are generated.

The variable `b` was incorrectly stored as:

    b = '???'

It should be generated as a secure random integer:

    import secrets
    b = secrets.randbelow(p - 2) + 1

The undefined `randint` call used for `a` was also replaced with a valid random-number generation method.

## Step 2 – Run the Corrected Script

Move into the `CRYPTO` directory:

    cd CRYPTO

Run the corrected script:

    python3 encryption.py

After running the script, inspect the generated message:

    cat message.txt

The file contains Diffie-Hellman values such as:

    p
    A
    b
    encrypted message

The important vulnerability is that the private value `b` has been exposed.

## Step 3 – Calculate the Shared Secret

In Diffie-Hellman, the shared secret can be calculated using:

    shared = pow(A, b, p)

Since `A`, `b`, and `p` are available, the shared secret can be recovered.

## Step 4 – Recover the Encryption Key

The encryption uses the shared secret modulo 256:

    key = shared % 256

The encrypted message is stored as hexadecimal data.

Convert the hexadecimal ciphertext into bytes:

    ciphertext = bytes.fromhex(ciphertext_hex)

## Step 5 – XOR the Ciphertext

Decrypt the ciphertext using the recovered key:

    plaintext = bytes(c ^ key for c in ciphertext)
    print(plaintext.decode())

This reveals the flag.

## Complete Python Solution

    p = int(p)
    A = int(A)
    b = int(b)

    shared = pow(A, b, p)
    key = shared % 256

    ciphertext = bytes.fromhex(ciphertext_hex)
    plaintext = bytes(c ^ key for c in ciphertext)

    print(plaintext.decode())

## Why This Works

Diffie-Hellman depends on the private values remaining secret.

Normally, the private value `b` should never be exposed.

The shared secret is calculated as:

    shared = pow(A, b, p)

Because `b` was leaked, the attacker can calculate the shared secret.

The encryption key is then:

    key = shared % 256

The ciphertext is converted from hexadecimal to bytes and XORed with the recovered key.

The complete attack path is:

    Inspect encryption.py
            ↓
    Fix the random value generation
            ↓
    Run the Python script
            ↓
    Read message.txt
            ↓
    Find p, A, b and ciphertext
            ↓
    Calculate shared = pow(A, b, p)
            ↓
    Calculate key = shared % 256
            ↓
    Convert ciphertext from hex to bytes
            ↓
    XOR ciphertext with key
            ↓
    Recover the flag

## Commands Used

    cd CRYPTO
    python3 encryption.py
    cat message.txt

Python calculations:

    shared = pow(A, b, p)
    key = shared % 256
    ciphertext = bytes.fromhex(ciphertext_hex)
    plaintext = bytes(c ^ key for c in ciphertext)
    print(plaintext.decode())

## One-Line Solution

    print(bytes(c ^ (pow(A, b, p) % 256) for c in bytes.fromhex(ciphertext_hex)).decode())

## Flag

    picoCTF{dh_s3cr3t_0d1562ee}

## Tools Used

- Linux Terminal
- Python 3
- Diffie-Hellman
- Python `pow()`
- `bytes.fromhex()`
- XOR
- `secrets`

## Key Learning

- Diffie-Hellman private values must remain secret.
- Exposing a private exponent can break the shared-secret exchange.
- Python's `pow(a, b, mod)` performs modular exponentiation.
- `bytes.fromhex()` converts hexadecimal data into bytes.
- XOR can decrypt data when the encryption key is known.
- Python's `secrets` module should be used for secure random values.

## Final Solution

    message.txt
    ↓
    Recover p, A and leaked b
    ↓
    shared = pow(A, b, p)
    ↓
    key = shared % 256
    ↓
    Convert ciphertext from hex
    ↓
    XOR ciphertext with key
    ↓
    picoCTF{dh_s3cr3t_0d1562ee}
