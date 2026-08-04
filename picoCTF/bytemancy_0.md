# bytemancy 0
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
This challenge tests basic knowledge of **ASCII encoding**. The remote service displays the decimal ASCII values of one or more characters and asks for the corresponding text representation.
The goal is to correctly convert the given ASCII decimal values into characters and send the answer back to the server.

## Approach
First, I connected to the remote service using Netcat.
```bash
nc candy-mountain.picoctf.net 53512
```
The server displayed the following prompt:
```text
Send me ASCII DECIMAL 101, 101, 101, side-by-side, no space.
```
Initially, I misunderstood the question and replied with the binary representation:
```text
101101101
```
The server rejected the answer because it expected the **ASCII character**, not the binary value.
Looking up the ASCII table shows:
```text
101 → e
```
Since the value `101` appeared three times, the correct response was simply:
```text
eee
```
After sending the correct input, the server immediately revealed the flag.

## Commands Used
Connect to the challenge:
```bash
nc candy-mountain.picoctf.net 53512
```
ASCII lookup example:
```text
101 → e
```
Response:
```text
eee
```

## Flag
```text
picoCTF{pr1n74813_ch4r5_2f7a75e5}
```

## Tools Used
- Netcat (nc)
- Linux Terminal
- ASCII Table

## Key Learning
- Learned how ASCII decimal values map to printable characters.
- Understood the difference between **ASCII**, **binary**, and **decimal** representations.
- Practiced interacting with remote services using Netcat.
- Reinforced quick conversion between ASCII values and characters, a common skill in CTF challenges.
