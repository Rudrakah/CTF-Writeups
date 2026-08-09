# ASCII Numbers
| **Platform** | picoCTF |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | LT 'syreal' Jones |
| **Tool** | CyberChef |

## Challenge
The challenge asks:
```text
Convert the following string of ASCII numbers into a readable string:
```
The provided values are:
```text
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d
```
The important observation is that every value begins with:
```text
0x
```
This means the values are written in **hexadecimal**.
## Hints
The challenge provides two hints:
```text
CyberChef is a great tool for any encoding but especially ASCII.
```
and:
```text
Try CyberChef's 'From Hex' function
```
This tells us directly that we should convert the hexadecimal values into ASCII characters.
## Step 1 – Identify the Encoding
The first few values are:
```text
0x70 0x69 0x63 0x6f 0x43 0x54 0x46
```
Converting them from hexadecimal to ASCII:
```text
0x70 → p
0x69 → i
0x63 → c
0x6f → o
0x43 → C
0x54 → T
0x46 → F
```
This gives:
```text
picoCTF
```
The next value is:
```text
0x7b
```
which represents:
```text
{
```
So we can already see the beginning:
```text
picoCTF{
```
## Step 2 – Use CyberChef
Open CyberChef and select the operation:
```text
From Hex
```
Paste the hexadecimal values into the input.
CyberChef converts the hexadecimal data into readable ASCII text.
The resulting string is:
```text
picoCTF{45e11_n0_qu35710n5_111_t311_y3_n0_1135_445d4180}
```
## Step 3 – Understand the Conversion
The conversion works because hexadecimal values correspond to ASCII character codes.
For example:
```text
0x70 → p
0x69 → i
0x63 → c
0x6f → o
0x43 → C
0x54 → T
0x46 → F
0x7b → {
```
A few more examples from the flag:
```text
0x34 → 4
0x35 → 5
0x5f → _
0x6e → n
0x30 → 0
0x71 → q
0x75 → u
0x33 → 3
0x7d → }
```
Combining all converted characters produces the complete flag.
## Alternative Method – Linux/Python
We can also convert the hexadecimal values using Python.
Remove the `0x` prefixes and spaces, then use:
```python
bytes.fromhex("7069636f4354467b34356331315f6e305f7137353731306e355f316c6c5f743331315f79335f6e305f6c3133355f34343564343138307d").decode()
```
The output is:
```text
picoCTF{45e11_n0_qu35710n5_111_t311_y3_n0_1135_445d4180}
```

## Flag
```text
picoCTF{45e11_n0_qu35710n5_111_t311_y3_n0_1135_445d4180}
```

## Tools Used
- CyberChef
- From Hex
- ASCII
- Python
- Linux Terminal

## Key Learning
- Values beginning with `0x` are commonly hexadecimal.
- Hexadecimal can represent ASCII character codes.
- CyberChef's **From Hex** operation can quickly convert hexadecimal into readable text.
- Python's `bytes.fromhex()` can perform the same conversion from the command line.
- In CTFs, recognizing the encoding format is often the main step toward solving the challenge.

## Final Solution
The entire solution can be summarized as:

```text
Receive hexadecimal values
        ↓
Notice the 0x prefix
        ↓
Recognize hexadecimal encoding
        ↓
Open CyberChef
        ↓
Select "From Hex"
        ↓
Paste the hexadecimal values
        ↓
Hexadecimal → ASCII
        ↓
picoCTF{45e11_n0_qu35710n5_111_t311_y3_n0_1135_445d4180}
        ↓
Submit
        ↓
Correct flag!
```
