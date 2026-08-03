# Log Hunt

| **Platform** | picoCTF(CyLab) |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge provides a log file containing scattered pieces of a flag. Some fragments appear multiple times, so duplicate entries must be ignored. The objective is to reconstruct the original flag from the log.

## Approach
I downloaded the provided `server.log` file and searched for all lines containing the keyword `FLAGPART` using `grep`.

```bash
grep "FLAGPART" server.log
```
The output contained several repeated flag fragments:
```text
picoCTF{us3_
y0urlinux_
sk1lls_
cedfa5fb}
```
Ignoring the duplicate entries and joining the unique fragments in the correct order produced the complete flag.

## Terminal Output
```bash
$ grep "FLAGPART" server.log

picoCTF{us3_
y0urlinux_
sk1lls_
cedfa5fb}
```

## Flag
```text
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```

## Tools Used
- Linux Terminal
- grep
  
## Key Learning
- Learned how to filter log files using `grep`.
- Understood how to search for specific patterns in large log files.
- Practiced extracting useful information while ignoring duplicate entries.
- Learned that log analysis is an important skill in cybersecurity and incident investigation.
