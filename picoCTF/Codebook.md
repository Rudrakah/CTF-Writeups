# Codebook

**Platform**  picoCTF(Cylab)
**Category**  General Skills 
**Difficulty**  Easy 

## Challenge
The challenge provides two files:
- `code.py`
- `codebook.txt`
The goal is to run the Python script using the provided codebook to recover the hidden flag.
The challenge description specifies that both files must be placed in the same directory before executing the script.
The hints also mention that there is **no need to reverse engineer** the `str_xor` function.

## Approach
First, I downloaded both files using the provided links inside the picoCTF WebShell.
wget https://artifacts.picoctf.net/c/1/code.py
wget https://artifacts.picoctf.net/c/1/codebook.txt
To verify that both files were downloaded successfully, I listed the current directory.
ls
The output confirmed that both `code.py` and `codebook.txt` were present.
Since the challenge instructions clearly stated to run the Python script in the same directory as the codebook, I executed:
python3 code.py
The script automatically read the contents of `codebook.txt`, performed the required decoding, and printed the flag.

## Terminal Output
$ wget https://artifacts.picoctf.net/c/1/code.py
$ wget https://artifacts.picoctf.net/c/1/codebook.txt
$ ls
code.py
codebook.txt
$ python3 code.py
picoCTF{c0d3b00k_455157_d9aa2df2}

## Flag
picoCTF{c0d3b00k_455157_d9aa2df2}

## Tools Used
- picoCTF WebShell
- wget
- Python 3

## Key Learning
- How to download files using `wget`.
- How to verify downloaded files using the `ls` command.
- How to execute a Python script from the terminal.
- Carefully reading the challenge description and hints can save unnecessary reverse engineering effort.
