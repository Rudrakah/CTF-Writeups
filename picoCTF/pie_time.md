# PIE TIME
| **Platform** | picoCTF 2025 |
| **Category** | Binary Exploitation |
| **Difficulty** | Easy |
| **Challenge** | PIE TIME |
| **Topic** | PIE / Address Randomization |

## Challenge

The challenge provides a binary exploitation service that can be accessed using netcat.

The main challenge is dealing with PIE, where the program's addresses can change between executions.

## Step 1 – Connect to the Server

Connect using:

nc rescued-float.picoctf.net 51115

The server displays the address of `main`.

Example:

Address of main: 0x616d48d822a7

It then asks for an address.

## Step 2 – Understand the Hint

The hint says:

"Can you figure out what changed between the address you found locally and in the server output?"

This points toward PIE (Position Independent Executable).

With PIE enabled, the program can be loaded at a different base address each time.

Therefore, an address obtained from the local binary cannot simply be used directly against the remote server.

## Step 3 – Compare the Addresses

First inspect the binary locally and find the address of `main`.

Then connect to the remote instance and check the address printed by the server.

The important observation is that the address of `main` is different because of PIE/ASLR.

The offset between functions remains consistent, while the base address changes.

## Step 4 – Provide the Correct Address

The remote server provides the runtime address of `main`.

Convert the required address into the format expected by the program.

For example, the address:

0x616d48d822a7

can be represented as bytes using little-endian ordering.

The resulting input is then supplied to the server.

## Step 5 – Get the Flag

After providing the correct address, the program reveals the flag:

picoCTF{b4s1c_p05t1t10n_1nd3p3nd3nc3_f8845f06}

## Commands Used

Connect to the challenge:

nc rescued-float.picoctf.net 51115

Inspect the binary locally:

file <binary>

Check security properties:

checksec --file=<binary>

Find symbols:

nm <binary> | grep main

## Key Learning

- PIE stands for Position Independent Executable.
- PIE causes program addresses to change when the binary is loaded.
- ASLR randomizes memory locations at runtime.
- Local addresses should not automatically be assumed to match remote addresses.
- The runtime address provided by the challenge can be used to determine the correct address needed for exploitation.
- Endianness matters when supplying addresses as raw bytes.

## Final Solution

Connect to the remote service using netcat.

↓

Read the runtime address of `main`.

↓

Compare it with the locally obtained address.

↓

Understand that PIE changes the binary's load address.

↓

Use the remote runtime address instead of blindly using the local address.

↓

Provide the correctly formatted address.

↓

The server reveals the flag.

## Flag

picoCTF{b4s1c_p05t1t10n_1nd3p3nd3nc3_f8845f06}

## One-Line Solution

Use the runtime `main` address provided by the PIE-enabled remote binary instead of relying on the local address.
