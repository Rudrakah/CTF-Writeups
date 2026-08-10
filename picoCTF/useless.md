# useless
| **Platform** | picoCTF 2023 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | Loic Shema |
| **Tags** | man |

## Challenge
The challenge says:

```text
There's an interesting script in the user's home directory.

The work computer is running SSH. We've been given a script which performs some basic calculations, explore the script and find a flag.
```

The challenge provides SSH access with:

```text
Hostname: saturn.picoctf.net
Port: 59406
Username: picoplayer
Password: password
```

The important clue is:

```text
explore the script and find a flag
```

The challenge also has the `man` tag, which means we should inspect the manual page associated with the script.

## Step 1 – Connect to the Server

Use SSH:

```bash
ssh -p 59406 picoplayer@saturn.picoctf.net
```

Enter the password:

```text
password
```

After connecting, we get:

```text
picoplayer@challenge:~$
```

## Step 2 – List the Files

First check the home directory:

```bash
ls
```

We should find a script named:

```text
useless
```

Check its permissions and details:

```bash
ls -l useless
```

## Step 3 – Examine the Script

The challenge says that the script performs basic calculations.

We can inspect it using:

```bash
cat useless
```

The script contains options for:

```text
add
sub
mul
div
```

The code performs simple calculations based on the arguments supplied.

For example, the script supports commands such as:

```bash
./useless add 1 2
```

and:

```bash
./useless mul 2 3
```

However, the challenge is not actually about the calculations.

The important clue is the `man` tag.

## Step 4 – Check the Manual Page

Run:

```bash
man useless
```

This opens the manual page for the script.

The manual contains information such as:

```text
useless -- This is a simple calculator script

SYNOPSIS
    useless, [add sub mul div] number1 number2

DESCRIPTION
    Use the useless macro to make simple calculations like addition,
    subtraction, multiplication and division.
```

It also provides examples:

```text
./useless add 1 2
./useless mul 2 3
./useless div 6 3
./useless sub 6 5
```

## Step 5 – Look Carefully at the Manual

The important part is that the manual page contains information that is not necessary for using the calculator.

The flag is hidden inside the manual page.

While viewing:

```bash
man useless
```

look through the entire page.

The flag appears as:

```text
picoCTF{us3l3ss_ch4113ng3_3xpl0it3d_5136}
```

This is why the challenge is tagged with:

```text
man
```

The solution is simply to inspect the provided manual page.

## Step 6 – Exit the Manual

If the manual is opened using `man`, press:

```text
q
```

to quit the manual viewer.

## Why This Works

The challenge gives us a script called:

```text
useless
```

and tells us to explore it.

Instead of only running the script, we should also check whether a manual page exists:

```bash
man useless
```

The manual page contains the hidden flag.

This is a common CTF lesson:

```text
Don't only execute a program.
Inspect its documentation and related files too.
```

## Important Commands

Connect to the server:

```bash
ssh -p 59406 picoplayer@saturn.picoctf.net
```

List files:

```bash
ls
```

Inspect the script:

```bash
cat useless
```

Check the manual:

```bash
man useless
```

Exit the manual:

```text
q
```

## Flag

```text
picoCTF{us3l3ss_ch4113ng3_3xpl0it3d_5136}
```

## Tools Used

- SSH
- Linux Terminal
- `ls`
- `cat`
- `man`

## Key Learning

- Always inspect files provided by a CTF challenge.
- Check whether a program has a manual page using `man`.
- Documentation can contain useful information or even hidden flags.
- Do not assume a challenge is about the obvious functionality of a program.
- The `man` command is useful for understanding Linux programs and commands.

## Final Solution

```text
Connect using SSH
        ↓
ssh -p 59406 picoplayer@saturn.picoctf.net
        ↓
Enter password: password
        ↓
ls
        ↓
Find useless
        ↓
Inspect the script
        ↓
cat useless
        ↓
Notice it is a calculator script
        ↓
Check the manual
        ↓
man useless
        ↓
Read the manual page carefully
        ↓
Find the hidden flag
        ↓
picoCTF{us3l3ss_ch4113ng3_3xpl0it3d_5136}
``` 
