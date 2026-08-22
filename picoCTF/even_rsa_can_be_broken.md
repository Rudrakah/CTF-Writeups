# EVEN RSA CAN BE BROKEN???
| **Platform** | picoCTF 2025 |
| **Category** | Cryptography |
| **Difficulty** | Easy |
| **Author** | Michael Crotty |
| **Topic** | RSA / Weak Prime Generation / Factoring |

## Challenge

The challenge is an RSA cryptography problem.

The hints are:

    How much do we trust randomness?
    Notice anything interesting about N?
    Try comparing N across multiple requests

The important observation is that the RSA modulus `N` is even.

Normally:

    N = p × q

where `p` and `q` are large prime numbers.

If `N` is even, one of the factors must be:

    p = 2

Therefore:

    q = N // 2

This makes the RSA key easy to break.

## Step 1 – Observe N

The challenge provides values similar to:

    N = ...
    e = 65537
    ciphertext = ...

Check whether `N` is even:

    N % 2

If the result is:

    0

then `N` is divisible by 2.

## Step 2 – Factor N

Because `N` is even:

    p = 2
    q = N // 2

Therefore:

    N = 2 × q

## Step 3 – Calculate Phi

For RSA:

    phi(N) = (p - 1)(q - 1)

Calculate:

    phi = (p - 1) * (q - 1)

## Step 4 – Calculate the Private Key

The public exponent is:

    e = 65537

The private exponent is:

    d = e⁻¹ mod phi

In Python:

    from Crypto.Util.number import inverse

    d = inverse(e, phi)

## Step 5 – Decrypt

RSA decryption is:

    m = c^d mod N

In Python:

    m = pow(c, d, N)

Convert the decrypted integer into bytes:

    from Crypto.Util.number import long_to_bytes

    print(long_to_bytes(m))

## Step 6 – Complete Python Solver

    from Crypto.Util.number import inverse, long_to_bytes

    N = YOUR_N
    e = 65537
    c = YOUR_CIPHERTEXT

    p = 2
    q = N // 2

    phi = (p - 1) * (q - 1)
    d = inverse(e, phi)

    m = pow(c, d, N)

    print(long_to_bytes(m))

Replace `YOUR_N` and `YOUR_CIPHERTEXT` with the values received from the challenge.

## Why This Works

RSA relies on the difficulty of factoring:

    N = p × q

when `p` and `q` are large random primes.

Here, `N` is even, so:

    p = 2

and:

    q = N // 2

Once both factors are known, we can calculate:

    phi(N)

Then calculate the private exponent:

    d = inverse(e, phi)

Finally decrypt:

    m = pow(c, d, N)

This reveals the original plaintext and the flag.

## Important RSA Concepts

### RSA Modulus

    N = p × q

### Euler's Totient

    phi(N) = (p - 1)(q - 1)

### Private Exponent

    d = e⁻¹ mod phi(N)

### RSA Decryption

    m = c^d mod N

### Integer to Bytes

    long_to_bytes(m)

converts the decrypted integer into readable bytes.

## Commands / Code Used

Start Python:

    python3

Import required functions:

    from Crypto.Util.number import inverse, long_to_bytes

Check whether N is even:

    N % 2

Factor N:

    p = 2
    q = N // 2

Calculate phi:

    phi = (p - 1) * (q - 1)

Calculate private key:

    d = inverse(e, phi)

Decrypt:

    m = pow(c, d, N)

Convert to bytes:

    print(long_to_bytes(m))

## One-Line Solution

    from Crypto.Util.number import inverse,long_to_bytes; p=2; q=N//2; phi=(p-1)*(q-1); d=inverse(e,phi); print(long_to_bytes(pow(c,d,N)))

## Complete Solution

    Connect to the challenge
            ↓
    Receive N, e and ciphertext
            ↓
    Notice N is even
            ↓
    p = 2
            ↓
    q = N // 2
            ↓
    phi = (p - 1)(q - 1)
            ↓
    d = inverse(e, phi)
            ↓
    m = pow(c, d, N)
            ↓
    Convert m to bytes
            ↓
    Read decrypted plaintext
            ↓
    Get the flag

## Flag

    picoCTF{tw0_1$_pr!m3dF98b648}

## Tools Used

- Python 3
- PyCryptodome
- RSA
- Modular Inverse
- `pow()`
- `long_to_bytes()`

## Key Learning

- RSA security depends on strong random prime numbers.
- An even RSA modulus immediately reveals that `2` is a factor.
- Once `p` and `q` are known, the RSA private key can be calculated.
- `inverse(e, phi)` calculates the modular inverse.
- `pow(c, d, N)` performs RSA decryption.
- Weak randomness or predictable prime generation can completely break RSA.

## Final Solution

    N is even
       ↓
    p = 2
       ↓
    q = N // 2
       ↓
    phi = (p - 1)(q - 1)
       ↓
    d = inverse(e, phi)
       ↓
    m = pow(c, d, N)
       ↓
    long_to_bytes(m)
       ↓
    picoCTF{tw0_1$_pr!m3dF98b648}
