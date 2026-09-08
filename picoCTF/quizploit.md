# Quizploit
| **Platform** | picoCTF 2026 |
| **Category** | Binary Exploitation |
| **Difficulty** | Easy |
| **Author** | Aditya Sudhanshu |
| **Topic** | ROP / Binary Exploitation / Memory Addresses |

## Challenge

The challenge is a binary exploitation quiz containing 13 questions.

The source code and binary are provided, and the challenge instance can be accessed using netcat.

The goal is to answer all 13 questions correctly to receive the flag.

## Step 1 – Connect to the Challenge

Connect to the remote instance:

nc lonely-island.picoctf.net 49347

The program asks a series of binary exploitation questions and provides multiple-choice options.

## Step 2 – Answer the Questions

The questions can be solved by analyzing the provided source code and binary.

For example, one question asks:

What exploitation technique could bypass NX?

The correct answer is:

ROP

ROP stands for Return-Oriented Programming and can be used when NX prevents directly executing injected shellcode.

## Step 3 – Find the Address of win()

Another question asks:

What is the address of `win()` in hex?

The hint recommends using GDB or objdump.

The address found was:

0x401176

This address was entered into the quiz.

## Step 4 – Complete the Quiz

Continue answering the remaining questions using information from the source code, binary, and common binary exploitation concepts.

After answering all questions correctly, the terminal displays:

QUIZ COMPLETE!

PERFECT SCORE!

You got 13/13 questions correct!

## Step 5 – Get the Flag

After completing all 13 questions, the challenge prints the flag:

picoCTF{my_b1n@4y_3xp10it_fl@g_690b52e8}

## Commands Used

Connect to the instance:

nc lonely-island.picoctf.net 49347

Inspect the binary:

file <binary>

Find symbols:

nm <binary> | grep win

Find the address using objdump:

objdump -d <binary> | grep "<win>"

Open the binary with GDB:

gdb <binary>

## Key Learning

- NX prevents execution of code from non-executable memory regions.
- ROP can be used to bypass NX by reusing existing executable code.
- GDB and objdump can be used to inspect binaries.
- `nm` can help locate symbols such as `win()`.
- Function addresses are important in binary exploitation.
- Understanding common exploitation techniques helps solve binary exploitation quizzes.
- Careful analysis of source code and binaries can reveal the answers to exploitation questions.

## Final Solution

Connect to the challenge using netcat.

↓

Answer the binary exploitation questions.

↓

For the NX question, select `ROP`.

↓

Find the address of `win()` using GDB/objdump.

↓

`win()` address = `0x401176`

↓

Continue answering all questions.

↓

Complete all 13 questions correctly.

↓

The challenge prints the flag.

## Flag

picoCTF{my_b1n@4y_3xp10it_fl@g_690b52e8}

## One-Line Solution

Answer all 13 Quizploit questions correctly using binary analysis and exploitation concepts to obtain the flag.
