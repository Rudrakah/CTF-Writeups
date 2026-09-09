# format string 0
| **Platform** | picoCTF 2024 |
| **Category** | Binary Exploitation |
| **Difficulty** | Easy |
| **Challenge** | format string 0 |
| **Topic** | Format String Vulnerability |

## Challenge

The challenge provides a program where customers choose their favorite burgers. The goal is to use knowledge of format strings to make the customers happy and eventually obtain the flag.

Connect to the challenge instance using:

nc mimas.picoctf.net 65233

## Step 1 – Connect to the Server

Run:

nc mimas.picoctf.net 65233

The server displays a list of burgers and asks for a recommendation.

## Step 2 – First Customer

The first customer wants a giant bite.

Choose the appropriate burger:

Gr%114d_Cheese

The format specifier `%114d` causes the program to process the input in a special way and results in the required output for the first customer.

The server responds:

Good job! Patrick is happy!

## Step 3 – Second Customer

The second customer wants something outrageous.

The available burgers include:

Pe%to_Portobello
$outhwest_Burger
Cl%ssic_Che%s%steak

The important option is:

Cl%ssic_Che%s%steak

The `%s` format specifier causes the program to interpret memory as a string, triggering the format-string vulnerability.

Enter:

Cl%ssic_Che%s%steak

The program then reveals the flag.

## Flag

picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_f89c1405}

## Commands Used

Connect to the challenge:

nc mimas.picoctf.net 65233

## Key Learning

- Format string vulnerabilities occur when user-controlled input is interpreted as a format string.
- `%d` is used for integer formatting.
- `%s` is used for string formatting.
- Malicious format specifiers can cause unexpected memory access.
- Carefully crafted format strings can manipulate program behavior.
- `nc` can be used to interact with remote CTF challenge services.

## Final Solution

Connect to the remote server.

↓

Select `Gr%114d_Cheese` for the first customer.

↓

Select `Cl%ssic_Che%s%steak` for the second customer.

↓

The format string vulnerability is triggered.

↓

The program prints the flag.

## One-Line Solution

nc mimas.picoctf.net 65233 → Gr%114d_Cheese → Cl%ssic_Che%s%steak → get the flag

## Flag

picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_f89c1405}
