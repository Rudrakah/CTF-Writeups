# Bytemancy 2
| **Platform**   | picoCTF 2026                      |
| **Category**   | General Skills                    |
| **Difficulty** | Medium                            |
| **Author**     | LT 'syreal' Jones                 |
| **Topic**      | Raw Bytes / Network Communication |

## Challenge

The challenge says:

```text
Can you conjure the right bytes? The program's source code can be downloaded here.
```

The hint says:

```text
There's no way to print these bytes
```

The second hint says:

```text
Use pwntools to send raw bytes over the network
```

The challenge asks us to send the HEX BYTE `0xFF` three times, side-by-side, with no spaces.

## Step 1 – Understand the Requirement

The server asks:

```text
Send me the HEX BYTE 0xFF 3 times, side-by-side, no space.
```

We need to send three raw `0xFF` bytes:

```text
FF FF FF
```

But there should be:

```text
No spaces
```

So the actual bytes sent must be:

```text
0xFF 0xFF 0xFF
```

followed by a newline.

We cannot simply type:

```text
ffffff
```

because that sends ASCII characters instead of the actual `0xFF` bytes.

## Step 2 – Why Normal `printf` Is Not Enough

The challenge specifically says:

```text
There's no way to print these bytes
```

The problem is that `0xFF` is not a normal printable ASCII character.

Instead, we need to generate the raw byte values directly.

Python can do this using:

```python
sys.stdout.buffer.write()
```

For example:

```python
python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff\n')"
```

This generates the actual bytes:

```text
FF FF FF
```

without converting them into ASCII text.

## Step 3 – Connect to the Challenge

The challenge instance provides the network service:

```text
lonely-island.picoctf.net
```

with port:

```text
59386
```

We can pipe the raw bytes directly into `nc`:

```bash
python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff\n')" | nc lonely-island.picoctf.net 59386
```

The important part is:

```python
b'\xff\xff\xff\n'
```

Here:

* `\xff` = hexadecimal byte `FF`
* Three `\xff` values = three `0xFF` bytes
* `\n` = newline to submit the input

## Step 4 – Alternative Method Using printf

We can also use Bash's `printf` to generate the raw bytes:

```bash
printf '\xff\xff\xff\n' | nc lonely-island.picoctf.net 59386
```

This is the method shown in the terminal.

The server receives the three raw bytes:

```text
FF FF FF
```

and accepts the answer.

## Step 5 – Server Response

After sending the correct bytes, the server responds with:

```text
picoCTF{3ff5_4_d4yz_c689238e}
```

The challenge page confirms:

```text
Correct flag!
```

## Important Concept – Raw Bytes

There is an important difference between:

```text
FF
```

and:

```text
0xFF
```

when sending data.

Typing:

```text
FF
```

normally sends the ASCII bytes representing the characters `F` and `F`.

But:

```text
\xff
```

represents the actual byte:

```text
11111111
```

or:

```text
0xFF
```

Therefore, to send three actual `0xFF` bytes, we use:

```python
b'\xff\xff\xff'
```

## Why Python Works

Python allows us to construct a bytes object directly:

```python
b'\xff\xff\xff'
```

The prefix:

```text
b
```

means that the value is a bytes object rather than a normal string.

Then:

```python
sys.stdout.buffer.write()
```

writes those bytes directly to standard output without performing text encoding.

## Commands Used

Connect to the service:

```bash
nc lonely-island.picoctf.net 59386
```

Send raw bytes using Python:

```bash
python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff\n')" | nc lonely-island.picoctf.net 59386
```

Send raw bytes using `printf`:

```bash
printf '\xff\xff\xff\n' | nc lonely-island.picoctf.net 59386
```

## Flag

```text
picoCTF{3ff5_4_d4yz_c689238e}
```

## Tools Used

* Linux Terminal
* Python 3
* `printf`
* `nc` / Netcat
* Raw bytes
* Hexadecimal

## Key Learning

* ASCII text and raw bytes are different.
* `0xFF` is a raw byte with the value `255`.
* Typing `FF` does not send the byte `0xFF`.
* Python's `bytes` objects can be used to construct raw byte sequences.
* `sys.stdout.buffer.write()` can send raw bytes without text encoding.
* `printf` can also generate hexadecimal byte sequences.
* Netcat can pipe raw bytes directly to a network service.

## Final Solution

```text
Start the challenge
        ↓
Read the required input
        ↓
Need to send 0xFF three times
        ↓
Cannot send printable ASCII
        ↓
Generate raw bytes using Python/printf
        ↓
Send FF FF FF with no spaces
        ↓
Pipe the bytes into nc
        ↓
Server accepts the input
        ↓
Flag
```

## One-Line Solution

```bash
printf '\xff\xff\xff\n' | nc lonely-island.picoctf.net 59386
```
