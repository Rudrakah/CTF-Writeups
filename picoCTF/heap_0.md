# Heap 0
| **Platform** | picoCTF 2024 |
| **Category** | Binary Exploitation |
| **Difficulty** | Easy |
| **Author** | Abxs, priorityQ |
| **Topic** | Heap Overflow |

## Challenge

The challenge asks us to find the flag by exploiting a heap-based buffer overflow.

The challenge provides a binary and source code and gives a remote service to connect to.

## Step 1 – Connect to the Challenge

Connect to the remote instance using:

nc tethys.picoctf.net 49443

The program provides these options:

1. Print Heap
2. Write to buffer
3. Print safe_var
4. Print Flag
5. Exit

## Step 2 – Inspect the Heap

Choose option 1:

1

The program displays the heap layout and shows the addresses of the variables.

Example:

[*] Address --> Heap Data
[*] 0x572683af92b0 --> pico
[*] 0x572683af92d0 --> bico

The hint tells us to determine which part of the heap we control and how far it is from `safe_var`.

## Step 3 – Exploit the Buffer Overflow

Choose option 2:

2

The program asks for data to write into the buffer.

Provide a long input such as:

AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAX

The input overflows the intended buffer and modifies data located next to it on the heap.

This demonstrates that the program does not properly limit the amount of data written to the buffer.

## Step 4 – Print the Flag

After corrupting the heap data, choose option 4:

4

The program responds:

YOU WIN

and prints the flag.

## Flag

picoCTF{my_first_heap_overflow_0c473fe8}

## Commands Used

Connect to the challenge:

nc tethys.picoctf.net 49443

Inspect the binary:

file <binary>

Inspect security protections:

checksec --file=<binary>

Analyze the binary:

gdb <binary>

## Why This Works

The program stores multiple variables next to each other on the heap.

The buffer-writing functionality does not properly check the length of the input.

By supplying more data than the buffer can hold, we overwrite adjacent heap data.

The overwritten value changes the program's state so that the flag condition becomes true.

Selecting option 4 then causes the program to print the flag.

## Key Learning

- Heap memory contains dynamically allocated data.
- A buffer overflow can overwrite adjacent memory.
- Heap overflows can modify nearby variables.
- Memory addresses can be inspected using the program's heap-printing functionality.
- Input length must always be properly validated.
- `gdb` and `checksec` are useful for analyzing binary exploitation challenges.

## Complete Solution

Connect to the server

↓

nc tethys.picoctf.net 49443

↓

Choose option 1

↓

Inspect the heap addresses

↓

Choose option 2

↓

Send an oversized input

↓

Heap data is overwritten

↓

Choose option 4

↓

YOU WIN

↓

Flag is printed

## One-Line Solution

nc tethys.picoctf.net 49443 → choose 1 → choose 2 → overflow the buffer → choose 4 → get the flag

## Flag

picoCTF{my_first_heap_overflow_0c473fe8}
