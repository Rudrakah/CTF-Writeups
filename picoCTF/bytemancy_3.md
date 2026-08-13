# Bytemancy 3
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | LT 'syreal' Jones |
| **Topic** | Binary Analysis / Symbol Table / Raw Bytes / pwntools |

## Challenge

The challenge says:

```text
Can you conjure the right bytes? The program's source code can be downloaded here and the compiled spellbook binary can be downloaded here.

Connect to the program with netcat:

$ nc green-hill.picoctf.net 61982
```

The program asks us to provide the raw addresses of hidden procedures.

The hints are:

```text
1. objdump -t spellbook reveals the symbol table.
2. Send the addresses as 4 raw bytes in little-endian order.
3. pwnlib.util.packing.p32() simplifies crafting the payloads.
```

The main task is:

```text
Find the addresses of the required procedures
        ↓
Convert each address to 4-byte little-endian format
        ↓
Send the raw bytes to the server
        ↓
Receive the flag
```

## Step 1 – Connect to the Server

The challenge gives:

```bash
nc green-hill.picoctf.net 61982
```

Run:

```bash
nc green-hill.picoctf.net 61982
```

The server asks for procedure addresses such as:

```text
[1/3] Send the 4-byte little-endian address for procedure 'ember_sigil'.
```

We therefore need to discover the addresses from the binary.

## Step 2 – Examine the Binary

The first hint tells us:

```text
objdump -t spellbook reveals the symbol table.
```

Run:

```bash
objdump -t spellbook
```

Search the output for the requested procedure names.

The relevant addresses are:

```text
ember_sigil   → 0x08049176
glyph_conflux → 0x0804919a
astral_spark  → 0x080491c1
binding_word  → 0x080491e3
```

These addresses are 32-bit addresses.

## Step 3 – Understand Little-Endian

The challenge requires:

```text
4 raw bytes in little-endian order
```

For example:

```text
0x08049176
```

is represented as:

```text
08 04 91 76
```

In little-endian order:

```text
76 91 04 08
```

Therefore the raw bytes are:

```text
\x76\x91\x04\x08
```

Similarly:

```text
ember_sigil
0x08049176 → 76 91 04 08

glyph_conflux
0x0804919a → 9a 91 04 08

astral_spark
0x080491c1 → c1 91 04 08

binding_word
0x080491e3 → e3 91 04 08
```

## Step 4 – Use Python to Pack the Addresses

Python's `struct.pack()` can convert the integer address into the required raw bytes.

The format:

```python
struct.pack("<I", address)
```

means:

```text
<  → little-endian
I  → unsigned 32-bit integer
```

For example:

```python
struct.pack("<I", 0x08049176)
```

produces:

```text
76 91 04 08
```

## Step 5 – Automated Solver

Instead of manually sending every address, we can write a Python script that:

1. Connects to the server.
2. Reads the requested procedure name.
3. Looks up its address.
4. Converts the address into 4-byte little-endian format.
5. Sends the bytes.
6. Repeats automatically.
7. Searches the output for the flag.
8. Prints the flag directly.

Create the solver:

```bash
nano solve.py
```

Use:

```python
import socket
import struct
import re

host = "green-hill.picoctf.net"
port = 61982

addresses = {
    "ember_sigil":  0x08049176,
    "glyph_conflux": 0x0804919a,
    "astral_spark":  0x080491c1,
    "binding_word": 0x080491e3
}

s = socket.create_connection((host, port))

output = b""
buf = b""

while True:
    data = s.recv(4096)

    if not data:
        break

    output += data
    buf += data

    # Check whether the flag has appeared
    flag = re.search(rb"picoCTF\{[^}]+\}", output)

    if flag:
        print("\n" + "=" * 50)
        print("FLAG:", flag.group(0).decode())
        print("=" * 50)
        break

    # Find the procedure requested by the server
    match = re.search(rb"procedure '([^']+)'", buf)

    if match:
        name = match.group(1).decode()

        if name in addresses:
            # Convert address to 4-byte little-endian
            raw = struct.pack("<I", addresses[name])

            print(f"[+] {name}")
            print(f"[+] Address : {hex(addresses[name])}")
            print(f"[+] Bytes   : {raw.hex()}")
            print("[+] Sending...")

            s.sendall(raw)

            # Clear buffer to avoid sending the same address twice
            buf = b""

s.close()
```

## Step 6 – Run the Automated Solver

Run:

```bash
python3 solve.py
```

The script automatically performs the complete interaction.

Example output:

```text
[+] ember_sigil
[+] Address : 0x8049176
[+] Bytes   : 76910408
[+] Sending...

[+] glyph_conflux
[+] Address : 0x804919a
[+] Bytes   : 9a910408
[+] Sending...

[+] astral_spark
[+] Address : 0x80491c1
[+] Bytes   : c1910408
[+] Sending...

[+] binding_word
[+] Address : 0x80491e3
[+] Bytes   : e3910408
[+] Sending...

==================================================
FLAG: picoCTF{0bjdump_m4g1c_b77d38cb}
==================================================
```

## One-Command Automated Solver

The entire solver can also be executed directly from the terminal without creating a file:

```bash
python3 - <<'PY'
import socket
import struct
import re

host = "green-hill.picoctf.net"
port = 61982

addresses = {
    "ember_sigil":  0x08049176,
    "glyph_conflux": 0x0804919a,
    "astral_spark": 0x080491c1,
    "binding_word": 0x080491e3
}

s = socket.create_connection((host, port))

output = b""
buf = b""

while True:
    data = s.recv(4096)

    if not data:
        break

    output += data
    buf += data

    flag = re.search(rb"picoCTF\{[^}]+\}", output)

    if flag:
        print("\nFLAG:", flag.group(0).decode())
        break

    match = re.search(rb"procedure '([^']+)'", buf)

    if match:
        name = match.group(1).decode()

        if name in addresses:
            raw = struct.pack("<I", addresses[name])
            print(f"[+] Sending {name}: {raw.hex()}")
            s.sendall(raw)
            buf = b""

s.close()
PY
```

This version directly prints:

```text
FLAG: picoCTF{0bjdump_m4g1c_b77d38cb}
```

## Why the Automated Solver Works

The server dynamically tells us which procedure address it wants.

For example:

```text
procedure 'ember_sigil'
```

The script extracts:

```text
ember_sigil
```

Then looks it up:

```python
addresses[name]
```

The address is packed using:

```python
struct.pack("<I", address)
```

The `<I` format converts the address into a 4-byte little-endian value.

The raw bytes are then sent with:

```python
s.sendall(raw)
```

The script repeats this for every procedure requested by the server.

Finally, it searches all received data using:

```python
re.search(rb"picoCTF\{[^}]+\}", output)
```

and prints the flag automatically.

## Important Concept – Symbol Table

The symbol table contains information about functions and other symbols inside the binary.

Running:

```bash
objdump -t spellbook
```

allows us to locate the hidden procedure addresses.

The important symbols are:

```text
ember_sigil
glyph_conflux
astral_spark
binding_word
```

## Important Concept – Little Endian

For example:

```text
0x08049176
```

Normal byte order:

```text
08 04 91 76
```

Little-endian byte order:

```text
76 91 04 08
```

Therefore:

```python
struct.pack("<I", 0x08049176)
```

produces:

```text
76 91 04 08
```

## Important Concept – Raw Bytes

The server does not want the address as text.

Incorrect:

```text
08049176
```

Correct:

```text
76 91 04 08
```

The second form contains the actual four bytes that represent the address in little-endian format.

## Addresses Used

```text
ember_sigil
0x08049176
→ 76 91 04 08

glyph_conflux
0x0804919a
→ 9a 91 04 08

astral_spark
0x080491c1
→ c1 91 04 08

binding_word
0x080491e3
→ e3 91 04 08
```

## Core Code

```python
match = re.search(rb"procedure '([^']+)'", buf)

if match:
    name = match.group(1).decode()

    if name in addresses:
        raw = struct.pack("<I", addresses[name])
        s.sendall(raw)
```

The important line is:

```python
raw = struct.pack("<I", addresses[name])
```

This converts the address into the required 4-byte little-endian format.

## Commands Used

Inspect the binary:

```bash
objdump -t spellbook
```

Connect manually:

```bash
nc green-hill.picoctf.net 61982
```

Run the automated solver:

```bash
python3 solve.py
```

## Final Solution

```text
Download the spellbook binary
        ↓
Run objdump -t spellbook
        ↓
Find the hidden procedure addresses
        ↓
ember_sigil   → 0x08049176
glyph_conflux → 0x0804919a
astral_spark  → 0x080491c1
binding_word  → 0x080491e3
        ↓
Connect to the server
        ↓
Read the requested procedure name
        ↓
Look up its address
        ↓
Convert address using struct.pack("<I", address)
        ↓
Send 4 raw little-endian bytes
        ↓
Repeat automatically
        ↓
Search server output for picoCTF{...}
        ↓
Print the flag
```

## Flag

```text
picoCTF{0bjdump_m4g1c_b77d38cb}
```

## Tools Used

- Linux Terminal
- `nc`
- `objdump`
- Python 3
- `socket`
- `struct`
- Regular Expressions
- Little-endian encoding
- Binary symbol table

## Key Learning

- `objdump -t` can reveal function addresses from a binary.
- Network services may require raw binary data instead of text.
- Little-endian means the least significant byte is sent first.
- `struct.pack("<I", address)` converts a 32-bit address into the required raw format.
- Python sockets can automate interactive CTF network challenges.
- Regular expressions can automatically detect server prompts and flags.
- Automating repetitive byte-level interactions makes CTF solving faster and less error-prone.
