# runme.py

**Platform:** picoCTF 2022

**Category:** General Skills

**Difficulty:** Easy

---

## Challenge
The challenge provides a Python script named `runme.py` and asks you to execute it to obtain the flag.
The description suggests downloading the script either through a web browser or using the `wget` command inside the picoCTF webshell.

## Approach
The challenge instructions mention using the `wget` command to download the Python script and then executing it with Python.
First, I copied the download link and downloaded the script inside the webshell using:

```bash
wget https://artifacts.picoctf.net/c/34/runme.py
```
After the download completed successfully, I executed the script with:
```bash
python3 runme.py
```
The script printed the flag directly to the terminal.
---

## Terminal Output
```bash
$ wget https://artifacts.picoctf.net/c/34/runme.py
Saving to: 'runme.py'

$ python3 runme.py
picoCTF{run_s4n1ty_run}
```

## Flag

```text
picoCTF{run_s4n1ty_run}

## Tools Used

- picoCTF WebShell
- wget
- Python 3

## Key Learning

- How to download files using `wget`.
- How to execute Python scripts from the terminal.
- Basic usage of the picoCTF WebShell.
- Following challenge instructions carefully can often lead directly to the solution.
