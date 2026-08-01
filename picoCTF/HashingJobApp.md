# HashingJobApp
 **Platform**  picoCTF(CyLab)
 **Category** General Skills 
 **Difficulty**  Easy 

## Challenge
The challenge provides a remote service that asks the user to generate **MD5 hashes** for different strings.
The objective is to calculate the correct MD5 hash for each given string and submit the answer before moving on to the next question. After successfully answering all the prompts, the service reveals the flag.
The hints suggest using either a command-line utility or an online tool to generate the hashes.

## Approach

I connected to the remote service using the provided Netcat (`nc`) command.
nc saturn.picoctf.net 57321
The server displayed a string and asked for its MD5 hash.

Example:
Please md5 hash the text between quotes, excluding the quotes:
'baby showers'


I generated the MD5 hash using an online MD5 generator (or a command-line tool) and submitted the result.
The service repeated this process several times with different strings.
'baby showers'
'Joan of Arc'
'construction workers'
After submitting the correct hash for every prompt, the service printed the flag.

## Terminal Output
$ nc saturn.picoctf.net 57321
Please md5 hash the text between quotes:
'baby showers'
Answer:
2c236af2a631160e18ec35119418c5ff
Correct.
Please md5 hash the text between quotes:
'Joan of Arc'
Answer:
19ba425a542946fcf13228d9ddd53139
Correct.
Please md5 hash the text between quotes:
'construction workers'
Answer:
00ae932c1c12ee89427c3efeeecf9533
Correct.
picoCTF{4ppl1c4710n_r3c31v3d_bf2ceb02}

## Flag
picoCTF{4ppl1c4710n_r3c31v3d_bf2ceb02}

## Tools Used
- picoCTF WebShell
- Netcat (`nc`)
- MD5 Generator / CyberChef

## Key Learning
- Learned how the **MD5 hashing algorithm** works.
- Understood the difference between **hashing** and **encryption**.
- Practiced interacting with remote services using Netcat.
- Learned how to quickly generate hashes using CyberChef or command-line tools.
