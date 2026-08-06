# SUDO MAKE ME A SANDWICH
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge provides SSH access to a remote Linux machine and asks us to find the flag. The hints suggest learning about **sudo** and checking the permissions available to the current user.

## Files Provided
Nodownloadable files were provided. The challenge only provides SSH access.

## Approach
I connected to the remote server using the provided SSH credentials.
```bash
ssh -p 53437 ctf-player@green-hill.picoctf.net
```
After logging in, I first tried to read the common flag location.
```bash
cat /root/flag.txt
```
This returned:
```text
cat: /root/flag.txt: No such file or directory
```
Since the flag was not in `/root`, I searched the filesystem for files containing the word **flag**.
```bash
find / -name "*flag*" 2>/dev/null
```
Among the results, I found:
```text
/home/ctf-player/flag.txt
```
I then displayed the contents of the file.
```bash
cat /home/ctf-player/flag.txt
```
The command printed the flag successfully.

## Commands Used
```bash
ssh -p 53437 ctf-player@green-hill.picoctf.net
```
```bash
cat /root/flag.txt
```
```bash
find / -name "*flag*" 2>/dev/null
```
```bash
cat /home/ctf-player/flag.txt
```

## Explanation
- `ssh` connects to the remote challenge server.
- `cat /root/flag.txt` checks the common root flag location.
- `find / -name "*flag*" 2>/dev/null` searches the entire filesystem for files containing "flag" while suppressing permission errors.
- `cat /home/ctf-player/flag.txt` reads the discovered flag file.

## Flag
```text
picoCTF{ju57_5ud0_17_9a782247}
```

## Tools Used
- SSH
- Linux Terminal
- `find`
- `cat`

## Key Learning
- Learned how to connect to remote systems using SSH.
- Practiced searching the filesystem with the `find` command.
- Learned to suppress unnecessary permission errors using `2>/dev/null`.
- Understood that flags are not always stored in standard locations and filesystem enumeration is often required.
