# Chrono
| **Platform** | picoCTF 2023 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Topic** | Linux / Cron |

## Challenge
The challenge asks:
```text
How to automate tasks to run at intervals on linux servers?
```
We are given SSH credentials to connect to a Linux server:
```text
Server: saturn.picoctf.net
Port: 63067
Username: picoplayer
Password: pYKku7iMsS
```
The main topic of this challenge is **cron**, which is a Linux service used to schedule commands and tasks at specific intervals.
## Step 1 – Connect to the Server
Use SSH with the provided hostname, port, and username:
```bash
ssh -p 63067 picoplayer@saturn.picoctf.net
```
When prompted for the password, enter:
```text
pYKku7iMsS
```
After successfully connecting, we get a Linux shell on the challenge server.
## Step 2 – Check the Cron Configuration
The challenge is about automating tasks at intervals, so the first thing to investigate is the system's cron configuration.
The system-wide cron configuration is stored in:
```text
/etc/crontab
```
Use:
```bash
cat /etc/crontab
```
The output contains:
```text
# picoCTF{Sch3DUL7NG_T45K3_L1NUX_7754e199}
```
The flag is directly present in the cron configuration file.
## Step 3 – Understand `/etc/crontab`
The `/etc/crontab` file is a system-wide configuration file used by `cron`.
Cron entries generally follow this structure:
```text
minute hour day month weekday user command
```
For example:
```text
*/5 * * * * root /path/to/script.sh
```
This would execute the specified command every five minutes.
In this challenge, simply inspecting `/etc/crontab` reveals the flag.
## Why This Works
The challenge asks:
```text
How to automate tasks to run at intervals on linux servers?
```
This points directly toward **cron**.
The relevant file is:
```text
/etc/crontab
```
Reading it with:
```bash
cat /etc/crontab
```
reveals the flag.
No exploitation or complicated commands are required.
## Commands Used

Connect to the server:
```bash
ssh -p 63067 picoplayer@saturn.picoctf.net
```
Read the cron configuration:
```bash
cat /etc/crontab
```

## Flag
```text
picoCTF{Sch3DUL7NG_T45K3_L1NUX_7754e199}
```

## Tools Used
- SSH
- Linux Terminal
- `cat`
- Cron
- `/etc/crontab`

## Key Learning
- **Cron** is used to schedule tasks on Linux systems.
- `/etc/crontab` contains system-wide cron jobs.
- `cat` can be used to read configuration files.
- Understanding common Linux configuration locations is useful during CTFs.
- Challenge descriptions often directly hint at the technology or file we need to investigate.

## Final Solution
The entire solution can be summarized as:

```text
Open Chrono
        ↓
Read the challenge description
        ↓
Identify the topic: cron
        ↓
SSH into the challenge server
        ↓
ssh -p 63067 picoplayer@saturn.picoctf.net
        ↓
Enter the provided password
        ↓
cat /etc/crontab
        ↓
Find the flag
        ↓
picoCTF{Sch3DUL7NG_T45K3_L1NUX_7754e199}
        ↓
Submit
        ↓
Correct flag!
```
