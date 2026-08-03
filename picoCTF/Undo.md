# Undo

| **Platform** | picoCTF(CyLab) |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge presents a flag that has been transformed using several common Linux text-processing commands. At each step, the service provides the transformed text and a hint describing what operation was applied.
The goal is to determine the correct Linux command that reverses each transformation and eventually recover the original flag.

## Approach
I launched the challenge instance and followed each transformation step carefully.

### Step 1
The text was Base64 encoded.
Reverse command:
```bash
base64 -d
```

### Step 2
The text was reversed.
Reverse command:
```bash
rev
```

### Step 3
Underscores (`_`) had been replaced with dashes (`-`).
Reverse command:
```bash
tr '-' '_'
```

### Step 4
Curly braces (`{}`) had been replaced with parentheses (`()`).
Reverse command:
```bash
tr '()' '{}'
```

### Step 5
The remaining text was encoded using ROT13.
Reverse command:
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
After completing all five steps correctly, the original flag was recovered.

## Terminal Output
```bash
base64 -d
rev
tr '-' '_'
tr '()' '{}'
tr 'A-Za-z' 'N-ZA-Mn-za-m'

picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_3a939318}
```

## Flag
```text
picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_3a939318}
```

## Tools Used
- Linux Terminal
- base64
- rev
- tr

## Key Learning
- Learned how to reverse common Linux text transformations.
- Practiced using `base64`, `rev`, and `tr`.
- Understood how multiple text transformations can be chained together.
- Carefully reading hints makes it easier to identify the correct command for each step.
