# Based
| **Platform** | picoCTF(CyLab) |
| **Category** | General Skills |
| **Difficulty** | Medium |

## Challenge
This challenge tests knowledge of common data encodings. A remote service continuously provides values encoded in different formats such as **Binary**, **Octal**, and **Hexadecimal**, and expects the decoded word within a limited amount of time.
The hints suggest using Python to perform the conversions quickly.

## Files Provided
No downloadable files were provided. The challenge is solved by interacting with the remote service.

## Approach
I connected to the challenge server using Netcat.
```bash
nc fickle-tempest.picoctf.net 52704
```
The server asked me to decode three different encodings.
### Step 1 – Binary
The server displayed:
```text
01110011 01110101 01100010 01101101 01100001 01110010 01101001 01101110 01100101
```
Converting each binary byte to ASCII gives:
```text
submarine
```

### Step 2 – Octal
The server then asked:
```text
o163 o165 o142 o155 o141 o162 o151 o156 o145
```
Converting the octal values to ASCII gives:
```text
submarine
```

### Step 3 – Hexadecimal
Finally, the server displayed:
```text
70656172
```
Splitting into hexadecimal bytes:
```text
70 65 61 72
```
Converting each byte to ASCII gives:
```text
pear
```
After submitting all three correct answers within the time limit, the server returned the flag.

## Commands Used
Connect to the challenge:
```bash
nc fickle-tempest.picoctf.net 52704
```
Example Python conversions:
Binary to text:
```python
bytes.fromhex("73").decode()
```
Hexadecimal to text:
```python
bytes.fromhex("70656172").decode()
```
Octal to text:
```python
''.join(chr(int(x,8)) for x in ["163","165","142","155","141","162","151","156","145"])
```

## Explanation
- Binary values were converted from base-2 into ASCII.
- Octal values were converted from base-8 into ASCII.
- Hexadecimal values were converted from base-16 into ASCII.
- After decoding each value correctly, the remote server revealed the flag.

## Flag
```text
picoCTF{learning_about_converting_values_acdCcfcCa}
```

## Tools Used
- Netcat (`nc`)
- Python 3
- Linux Terminal
- ASCII Table

## Key Learning
- Learned to recognize Binary, Octal, and Hexadecimal encodings.
- Practiced converting encoded values into ASCII characters.
- Used Python to automate base conversions efficiently.
- Reinforced working with interactive terminal-based CTF challenges under time constraints.
