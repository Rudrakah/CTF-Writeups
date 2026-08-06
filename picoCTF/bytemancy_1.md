# bytemancy 1
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
This challenge is a continuation of **bytemancy 0**. Instead of manually typing the required characters, the server asks for a character repeated a very large number of times.
The hint suggests using **Python** instead of manually copying and pasting the output.

## Approach
I connected to the remote service using Netcat.
```bash
nc foggy-cliff.picoctf.net 53432
```
The server displayed the following prompt:
```text
Send me ASCII DECIMAL 101 1751 times, side-by-side, no space.
```
From the ASCII table:
```text
101 → e
```
Instead of manually typing **1751** `e` characters, I used a short Python one-liner to generate the required output and piped it directly into the Netcat connection.

## Commands Used
Connect and automatically send the answer:
```bash
python3 -c 'print("e"*1751)' | nc foggy-cliff.picoctf.net 53432
```
Explanation:
- `python3 -c` executes a Python command directly from the terminal.
- `"e"*1751` generates the letter **e** repeated **1751** times.
- `print()` outputs the generated string.
- `|` pipes the output into the Netcat connection.
- `nc` sends the generated string directly to the challenge server.
The server accepted the response and immediately returned the flag.

## Flag
```text
picoCTF{h0w_m4ny_e's???_446bf6f1}
```

## Tools Used
- Netcat (nc)
- Python 3
- Linux Terminal
- ASCII Table

## Key Learning
- Learned how to automate repetitive tasks using Python.
- Practiced piping program output directly into another command.
- Understood how Python one-liners can simplify CTF challenges.
- Reinforced combining `python3` with `nc` for efficient interaction with remote services.
