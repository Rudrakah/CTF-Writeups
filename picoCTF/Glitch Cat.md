# Glitch Cat
 **Platform** picoCTF(cylab)
 **Category**  General Skills
 **Difficulty**  Easy

## Challenge
The challenge provides a Netcat (`nc`) service that prints a "glitched" version of the flag instead of displaying it directly.
The challenge hints are:
1. ASCII is one of the most common encodings used in programming.
2. The glitch output is valid Python.
3. Press **Ctrl + C** to terminate the connection.
The objective is to recover the original flag from the glitched output.

## Approach
I connected to the remote service using the provided Netcat command.
nc saturn.picoctf.net 54312
Instead of printing the complete flag, the server returned a Python expression similar to:
```python
'picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) +
chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}'
```

The `chr()` function converts an ASCII value into its corresponding character.

For example:
| Expression | Character |
|-----------|-----------|
| `chr(0x62)` | `b` |
| `chr(0x64)` | `d` |
| `chr(0x61)` | `a` |
| `chr(0x36)` | `6` |
| `chr(0x38)` | `8` |
| `chr(0x66)` | `f` |
| `chr(0x37)` | `7` |
| `chr(0x35)` | `5` |
Replacing every `chr()` call with its corresponding character reconstructs the complete flag.

## Terminal Output
$ nc saturn.picoctf.net 54312
'picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) +
chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}'

## Flag
picoCTF{gl17ch_m3_n07_bda68f75}

## Tools Used
- picoCTF WebShell
- basic ASCII

## Key Learning
- How to connect to a remote service using Netcat.
- The `chr()` function converts an ASCII value into a character.
- Python expressions can be evaluated to reconstruct hidden text.
- Reading challenge hints carefully often reveals the intended solution.
