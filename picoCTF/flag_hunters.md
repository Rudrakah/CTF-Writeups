# Flag Hunters
| **Platform** | picoCTF 2025 |
| **Category** | Reverse Engineering |
| **Difficulty** | Easy |
| **Author** | syreal |
| **Topic** | Python / Program Flow / Input Manipulation / Refactoring |

## Challenge

The challenge provides a program containing lyrics that jump between verses using a refrian-like mechanism.

The goal is to understand the program's control flow and find a way to make the hidden refrain print the flag.

The challenge description says:

    Lyrics jump from verses to the refrain kind of like a subroutine call.
    There's a hidden refrain this program doesn't print by default.
    Can you get it to print it?

The program's source code can be downloaded from the challenge.

## Step 1 – Inspect the Program

Download the source code and inspect it.

The program contains sections such as:

    [REFRAIN]
    We're flag hunters in the ether, lighting up the grid,
    No puzzle too dark, no challenge too hid.
    With every exploit we trigger, every byte we decrypt,
    We're chasing that victory, and we'll never quit.

The program also contains a `Crowd:` section and input handling that controls how the lyrics are processed.

## Step 2 – Understand the Program Flow

The important part of the challenge is that the program uses user input to control which part of the lyrics is printed.

The hints point toward:

    This program can easily get into undefined states.
    Don't be shy about Ctrl-C.

    Unsanitized user input is always good, right?

    Is there any syntax that is ripe for subversion?

This indicates that the input is not properly sanitized and can affect the program's control flow.

## Step 3 – Trigger the Hidden Refrain

By interacting with the program and providing input that changes the expected control flow, the hidden `[REFRAIN]` section can be reached.

The program then prints the refrain multiple times.

The important output eventually contains:

    The ether's ours to conquer, picoCTF{70637h3r_f0r3v3r_836f0788}

## Step 4 – Recover the Flag

The flag is directly visible in the program output:

    picoCTF{70637h3r_f0r3v3r_836f0788}

## Why This Works

The challenge is testing understanding of program flow and unsafe input handling.

The program treats user-controlled input as part of its internal processing without properly restricting it.

This allows the normal flow of the program to be manipulated so that the hidden refrain is executed.

The overall approach is:

    Download source code
            ↓
    Inspect the program
            ↓
    Understand the refrain/control-flow logic
            ↓
    Notice unsanitized input
            ↓
    Manipulate the input
            ↓
    Trigger [REFRAIN]
            ↓
    Read the hidden flag
            ↓
    Submit the flag

## Commands Used

Run the downloaded program using the appropriate command for the provided source file.

For example:

    python3 <source_file>

If the program enters an unexpected state, use:

    Ctrl+C

Then restart it and experiment with the input handling.

## One-Line Solution

    Manipulate the unsanitized input to trigger the hidden [REFRAIN] and read the flag.

## Flag

    picoCTF{70637h3r_f0r3v3r_836f0788}

## Tools Used

- Linux Terminal
- Python
- Source Code Analysis
- Ctrl+C
- Program Input Analysis

## Key Learning

- User-controlled input can affect program control flow when it is not properly sanitized.
- Reading source code is often the fastest way to understand a reverse-engineering challenge.
- Hidden functions or subroutines may be reachable through unexpected program states.
- Program output can reveal information that is not printed during normal execution.
- Understanding control flow is an important reverse-engineering skill.

## Final Solution

    Download the source code
            ↓
    Inspect the program
            ↓
    Analyze the input handling
            ↓
    Identify the unsanitized input
            ↓
    Manipulate the input
            ↓
    Trigger the hidden [REFRAIN]
            ↓
    Read the flag
            ↓
    picoCTF{70637h3r_f0r3v3r_836f0788}
