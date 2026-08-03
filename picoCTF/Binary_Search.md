# Binary Search
| **Platform** | picoCTF 2024 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge presents an interactive Binary Search Game over SSH. A random number between **1 and 1000** is chosen, and the player has only **10 guesses** to find it. After each guess, the server responds with either **Higher!** or **Lower!**, guiding the player toward the correct number.
The objective is to use the **Binary Search algorithm** to minimize the number of guesses and recover the flag.

## Approach
First, I connected to the remote challenge server using the provided SSH credentials.
```bash
ssh -p 53689 ctf-player@atlas.picoctf.net
```
Once connected, the game started and prompted for guesses.
Instead of guessing randomly, I used the **Binary Search** algorithm:
1. Start with the middle value (500).
2. If the server responds **Higher**, search the upper half.
3. If the server responds **Lower**, search the lower half.
4. Repeat until the correct number is found.

Example:

```text
500 → Lower
200 → Lower
50  → Higher
100 → Lower
80  → Lower
60  → Lower
53  → Higher
57  → Correct
```

The server then displayed the flag.

## Commands Used

```bash
ssh -p 53689 ctf-player@atlas.picoctf.net
```

## Flag
```text
picoCTF{g00d_gu355_1597707f}
```

## Tools Used
- Linux Terminal
- SSH

## Key Learning
- Learned how Binary Search efficiently narrows down a search space.
- Understood how to apply the algorithm in an interactive terminal challenge.
- Practiced connecting to remote services using SSH.
- Reinforced the importance of choosing optimal algorithms over brute force.
