# Serpentine
| **Platform** | picoCTF Beginner picoMini 2022 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Language** | Python |

## Challenge
The challenge says:
```text
Find the flag in the Python script!
```
A Python script is provided for download. The hints tell us to first run the script and then examine the source code using an editor such as `nano`.
The important hint is:
```text
The str_xor function does not need to be reverse engineered for this challenge.
```
This means we do not need to understand or break the XOR encryption. Instead, we need to find why the existing `print_flag()` function is not being called.

## File Name
The downloaded Python file is:
```text
serpentine.py
```
## Step 1 – Download the Python Script
Download the provided script from the challenge page.
If using the Webshell, the general command is:
```bash
wget <download-link>
```
Then check that the file exists:
```bash
ls
```
We should see:
```text
serpentine.py
```
## Step 2 – Run the Script
Run the Python script:
```bash
python3 serpentine.py
```
The program displays a menu similar to:
```text
a) Print encouragement
b) Print flag
c) Quit
What would you like to do? (a/b/c)
```
Choosing:
```text
b
```
does not print the flag.
Instead, the program says:
```text
Oops! I must have misplaced the print_flag function! Check my source code!
```
This is the important clue.
The program itself tells us to inspect the source code.
## Step 3 – Open the Source Code
Use `nano`:
```bash
nano serpentine.py
```
Search through the Python code.
We find a function called:
```python
def print_flag():
```
Inside it, the encrypted flag is decoded using the existing `str_xor()` function:
```python
def print_flag():
    flag = str_xor(flag_enc, 'enkidu')
    print(flag)
```
So the flag-printing function already exists.
The problem is that the main program does not call it when we select option `b`.
## Step 4 – Find the Problem
Near the bottom of the script, there is a menu loop.
The important section looks like:
```python
while True:
    print('a) Print encouragement')
    print('b) Print flag')
    print('c) Quit\n')

    choice = input('What would you like to do? (a/b/c) ')

    if choice == 'a':
        print_encouragement()

    elif choice == 'b':
        print('\n Oops! I must have misplaced the print_flag function! Check my source code!\n\n')
```
The problem is here:
```python
elif choice == 'b':
    print('\n Oops! I must have misplaced the print_flag function! Check my source code!\n\n')
```
Instead of calling:
```python
print_flag()
```
the program only prints the error message.
## Step 5 – Modify the Code
Change:
```python
elif choice == 'b':
    print('\n Oops! I must have misplaced the print_flag function! Check my source code!\n\n')
```
to:
```python
elif choice == 'b':
    print_flag()
```
Now the program will actually execute the existing `print_flag()` function.
## Step 6 – Save the File
While inside `nano`:
```text
Ctrl + X
```
Then confirm saving the changes.
After returning to the terminal, run:
```bash
python3 serpentine.py
```
## Step 7 – Print the Flag
Choose:
```text
b
```
The program now calls:
```python
print_flag()
```
The function performs:
```python
flag = str_xor(flag_enc, 'enkidu')
```
and prints the decoded flag.
## Why We Don't Need to Reverse Engineer `str_xor`
The challenge specifically tells us:
```text
The str_xor function does not need to be reverse engineered for this challenge.
```
The function is already implemented correctly.
It takes:
```text
flag_enc
```
and XORs it with the repeating key:
```text
enkidu
```
The important part is simply making sure that:
```python
print_flag()
```
gets executed.
Therefore, there is no need to manually calculate the XOR values.

## Important Code Change
### Before
```python
elif choice == 'b':
    print('\n Oops! I must have misplaced the print_flag function! Check my source code!\n\n')
```
### After
```python
elif choice == 'b':
    print_flag()
```

## Commands Used

Download:

```bash
wget <download-link>
```

Check files:

```bash
ls
```

Run the script:

```bash
python3 serpentine.py
```

Open the source:

```bash
nano serpentine.py
```

Run again after modification:

```bash
python3 serpentine.py
```

## Flag
The canonical picoCTF Serpentine solution is:
```text
picoCTF{7h3_r04d_l355_7r4v3l3d_aa2340b2}
```
Some third-party writeups show a different final suffix, but the source/writeup versions are inconsistent; use the flag produced by **your own challenge instance** if it differs. :contentReference[oaicite:0]{index=0}

## Tools Used
- Python 3
- `nano`
- Linux Terminal
- XOR function
- `wget`

## Key Learning
- Learn to read and modify Python source code instead of blindly running a program.
- A function can exist in a program without ever being called.
- The challenge did not require breaking the XOR algorithm.
- The vulnerability/problem was simply that the menu option for printing the flag was not connected to `print_flag()`.
- Always inspect source code when a program tells you to "check my source code."

## Final Solution
The entire solution can be summarized as:
```text
Download serpentine.py
        ↓
Run python3 serpentine.py
        ↓
Select option b
        ↓
Program gives an error
        ↓
Open serpentine.py
        ↓
Find print_flag()
        ↓
Find broken option b
        ↓
Replace error message with print_flag()
        ↓
Save the file
        ↓
Run python3 serpentine.py
        ↓
Select b
        ↓
Flag
```
