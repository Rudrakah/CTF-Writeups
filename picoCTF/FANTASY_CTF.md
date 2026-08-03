# FANTASY CTF
| **Platform** | picoCTF(CyLab) |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
This challenge is an interactive terminal-based adventure game designed to familiarize players with the Linux terminal and the basic rules followed during picoCTF competitions.
The objective is simply to connect to the remote service, complete the interactive story by selecting the appropriate options, and receive the flag at the end.

## Approach
I connected to the remote service using **Netcat (nc)**.

```bash
nc verbal-sleep.picoctf.net 62183
```
The server launched an interactive fantasy-style game where I progressed through the story by reading the prompts and selecting the available choices.
Throughout the game I:

- Followed the instructions shown on the screen.
- Selected the appropriate menu options whenever prompted.
- Continued through each stage of the story until reaching the final scene.
- Completed the game successfully.

After the final dialogue, the game congratulated me and displayed the flag.

## Commands Used
Connect to the challenge:
```bash
nc verbal-sleep.picoctf.net 62183
```

## Flag
```text
picoCTF{m1l13n1um_3d1710n_8d7ec7f5}
```

## Tools Used
- Netcat (nc)
- Linux Terminal

## Key Learning
- Learned how to connect to remote services using Netcat.
- Practiced interacting with terminal-based applications.
- Became familiar with navigating interactive command-line programs.
- Reinforced basic Linux terminal usage required for future picoCTF challenges.
