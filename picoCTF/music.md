# music
| **Platform** | picoCTF(CyLab) |
| **Category** | General Skills |
| **Difficulty** | Medium |

## Challenge
The challenge provides a file named `lyrics.txt` (or similar song file) and asks us to recover the hidden message. The hint suggests using the **Rockstar programming language**, indicating that the song is actually a Rockstar program rather than plain text.

## Files Provided
```text
lyrics.txt
```

## Approach
After opening the provided file, I noticed that it looked like song lyrics instead of source code. The hint mentioning **Rockstar** suggested that the lyrics should be interpreted as a Rockstar program.
Instead of reading the file manually, I used an online Rockstar interpreter and pasted the contents of the song into it.
The interpreter executed the Rockstar program and printed a sequence of decimal ASCII values.

```text
114 114 114 111 99 107 110 114 110 48 49 49 51 114
```
These numbers represent ASCII decimal values. Converting each decimal value to its corresponding ASCII character gives:

| Decimal | Character |
|---------:|:---------|
| 114 | r |
| 114 | r |
| 114 | r |
| 111 | o |
| 99 | c |
| 107 | k |
| 110 | n |
| 114 | r |
| 110 | n |
| 48 | 0 |
| 49 | 1 |
| 49 | 1 |
| 51 | 3 |
| 114 | r |
Combining the decoded characters produces:

```text
rrrocknrn0113r
```

Finally, I placed the decoded text inside the required picoCTF flag format.

## Commands / Tools Used
Rockstar Interpreter:

```text
Pico's a CTFFFFFFF
my mind is waitin
It's waitin

Put my mind of Pico into This
my flag is not found
put This into my flag
put my flag into Pico


shout Pico
shout Pico
shout Pico

My song's something
put Pico into This

Knock This down, down, down
put This into CTF

shout CTF
my lyric is nothing
Put This without my song into my lyric
Knock my lyric down, down, down

shout my lyric

Put my lyric into This
Put my song with This into my lyric
Knock my lyric down

shout my lyric

Build my lyric up, up ,up

shout my lyric
shout Pico
shout It

Pico CTF is fun
security is important
Fun is fun
Put security with fun into Pico CTF
Build Fun up
shout fun times Pico CTF
put fun times Pico CTF into my song

build it up

shout it
shout it

build it up, up
shout it
shout Pico
```

ASCII Conversion:

```text
114 114 114 111 99 107 110 114 110 48 49 49 51 114
        ↓
rrrocknrn0113r
```

## Explanation
- The provided song is actually a **Rockstar** program.
- Running the program outputs decimal ASCII values.
- Each decimal number is converted to its ASCII equivalent.
- The decoded string is wrapped inside the picoCTF flag format.

## Flag
```text
picoCTF{rrrocknrn0113r}
```

## Tools Used
- Rockstar Interpreter
- ASCII Table / CyberChef
- Linux Terminal (optional)

## Key Learning
- Learned about the **Rockstar programming language**, an esoteric language where song lyrics act as executable code.
- Practiced interpreting program output instead of reading source code directly.
- Reinforced converting decimal ASCII values into readable text.
- Learned that CTF challenges may hide data inside uncommon programming languages.
