# Specialer
| **Platform** | picoCTF 2023 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | LT 'syreal' Jones, et al. |
| **Topic** | Bash / SSH / Shell |

## Challenge

The challenge describes a restricted shell called **Specialer**:

```text
Reception of Special has been cool to say the least. That's why we made an exclusive version of Special, called Secure Comprehensive Interface for Affecting Linux Empirically Rad, or just "Specialer".

With Specialer, we really tried to remove the distractions from using a shell. Yes, we took out spell checking because of everybody's complaining.

But we think you will be excited about our new, reduced feature set for keeping you focused on what needs it the most.
```

The challenge provides SSH access:

```text
Server: saturn.picoctf.net
Port: 50981
Username: ctf-player
Password: 483e80d4
```

The hint is:

```text
What programs do you have access to?
```

The important observation is that this is a **restricted shell**. Normal commands such as `ls` and `cat` are not necessarily available.

However, shell built-ins such as `printf` and `echo` can still be used.

The goal is to use shell features such as **wildcards (`*`)**, **command substitution**, and **file expansion** to locate and read the flag.

## Step 1 – Connect to the Server

Use the SSH command provided by the challenge:

```bash
ssh -p 50981 ctf-player@saturn.picoctf.net
```

Enter the password:

```text
483e80d4
```

After logging in, the prompt appears as:

```text
Specialer$
```

This confirms that we are inside the restricted Specialer shell.

## Step 2 – Check What We Can Execute

The hint asks:

```text
What programs do you have access to?
```

Since normal commands may not work, try the shell builtin:

```bash
printf "%s\n" *
```

This expands the `*` wildcard and prints the names of files/directories in the current directory.

The output is:

```text
abra
ala
sim
```

So there are three entries:

```text
abra
ala
sim
```

## Step 3 – Explore the `ala` Directory

We cannot rely on `ls`, but shell globbing can still reveal directory contents.

Run:

```bash
printf "%s\n" ala/*
```

The shell expands `ala/*` before `printf` executes.

The output shows:

```text
ala/kazam.txt
ala/mode.txt
```

Therefore, we have found two files:

```text
ala/kazam.txt
ala/mode.txt
```

## Step 4 – Read the File Without `cat`

Normally we would use:

```bash
cat ala/kazam.txt
```

but `cat` is not available in the restricted shell.

Bash provides another way to read a file:

```bash
$(<file)
```

This performs command substitution using Bash's file-reading feature.

Therefore, we can use:

```bash
echo "$(<ala/kazam.txt)"
```

This reads the contents of `ala/kazam.txt` and passes the result to `echo`.

The output contains:

```text
return 0 picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_d5ef8b71}
```

The flag is the `picoCTF{...}` portion.

## Step 5 – Extract the Flag

The flag is:

```text
picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_d5ef8b71}
```

## Why This Works

The challenge uses a restricted shell, so many common Linux commands are unavailable.

Instead of depending on:

```text
ls
cat
```

we use Bash's built-in features.

### Wildcard Expansion

The command:

```bash
printf "%s\n" *
```

uses `*` to expand filenames in the current directory.

Similarly:

```bash
printf "%s\n" ala/*
```

reveals the contents of the `ala` directory.

### Reading Files with Bash

Bash can read a file using:

```bash
$(<filename)
```

Therefore:

```bash
echo "$(<ala/kazam.txt)"
```

reads and prints the contents of `kazam.txt` without requiring the `cat` command.

## Commands Used

Connect to the server:

```bash
ssh -p 50981 ctf-player@saturn.picoctf.net
```

List the current directory using wildcard expansion:

```bash
printf "%s\n" *
```

Explore the `ala` directory:

```bash
printf "%s\n" ala/*
```

Read the flag file:

```bash
echo "$(<ala/kazam.txt)"
```

## Important Concepts

### 1. Restricted Shell

A restricted shell limits the commands or functionality available to the user.

Instead of immediately trying to bypass it, first determine what shell features are still available.

### 2. Shell Globbing

The `*` character is expanded by the shell.

For example:

```bash
printf "%s\n" *
```

can display filenames without using `ls`.

### 3. Bash File Reading

Bash provides:

```bash
$(<file)
```

as a convenient way to read a file's contents.

For example:

```bash
echo "$(<ala/kazam.txt)"
```

### 4. Built-in Commands

`printf` and `echo` are shell built-ins, which means they can remain available even when external programs such as `ls` and `cat` are restricted.

## Final Solution

```text
SSH into the server
        ↓
Enter the restricted Specialer shell
        ↓
Use printf instead of ls
        ↓
printf "%s\n" *
        ↓
Find the ala directory
        ↓
printf "%s\n" ala/*
        ↓
Find ala/kazam.txt
        ↓
Use Bash file expansion instead of cat
        ↓
echo "$(<ala/kazam.txt)"
        ↓
Read the file contents
        ↓
Extract the picoCTF flag
```

## Flag

```text
picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_d5ef8b71}
```

## Tools Used

- Linux Terminal
- SSH
- Bash
- `printf`
- `echo`
- Shell globbing
- Command substitution

## Key Learning

- Restricted shells do not necessarily prevent all shell functionality.
- `printf` can be used instead of `ls` for filename enumeration.
- Wildcards such as `*` are expanded by the shell.
- Bash can read files using `$(<file)`.
- Shell built-ins can be extremely useful when external commands are unavailable.
- Always inspect the available shell functionality before assuming that a command is impossible.
