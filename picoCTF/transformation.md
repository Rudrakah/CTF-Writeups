# Transformation
| **Platform** | picoCTF 2021 |
| **Category** | Reverse Engineering |
| **Difficulty** | Easy |
| **Author** | madsS |
| **Topic** | Encoding / Python / Bit Manipulation |

## Challenge

The challenge provides an encoded value generated using:

"".join(chr((ord(flag[i]) << 8) + ord(flag[i+1])) for i in range(0, len(flag), 2))

The goal is to reverse this transformation and recover the original flag.

## Step 1 – Download the File

Download the challenge file named:

enc

Then verify that the file exists:

cat enc

## Step 2 – Understand the Encoding

The program processes the flag two characters at a time.

For every pair of characters, it performs:

ord(flag[i]) << 8

and then adds:

ord(flag[i+1])

So two 8-bit ASCII characters are combined into one larger value.

The transformation is:

first_character << 8 | second_character

## Step 3 – Reverse the Transformation

To recover the first character, shift the encoded value right by 8 bits:

value >> 8

To recover the second character, use:

value & 0xff

Then convert both values back into characters using chr().

## Step 4 – Decode Using Python

Run:

python3 -c 's=open("enc").read().strip(); print("".join(chr(ord(c)>>8)+chr(ord(c)&255) for c in s))'

This reverses the transformation for every encoded character.

## Step 5 – Recover the Flag

The decoded output is:

picoCTF{16_bits_inst34d_of_8_b7f62ca5}

## Why This Works

The original encoding combines two characters into one 16-bit value.

For example:

Character 1
    ↓
ord(character 1)
    ↓
shift left by 8 bits
    ↓
combine with Character 2
    ↓
encoded character

To reverse it:

encoded character
    ↓
ord(encoded character)
    ↓
value >> 8
    ↓
first character

and:

encoded character
    ↓
ord(encoded character)
    ↓
value & 0xff
    ↓
second character

This reconstructs the original flag two characters at a time.

## Commands Used

Check the encoded file:

cat enc

Decode the file:

python3 -c 's=open("enc").read().strip(); print("".join(chr(ord(c)>>8)+chr(ord(c)&255) for c in s))'

## One-Line Solution

python3 -c 's=open("enc").read().strip(); print("".join(chr(ord(c)>>8)+chr(ord(c)&255) for c in s))'

## Flag

picoCTF{16_bits_inst34d_of_8_b7f62ca5}

## Tools Used

- Linux Terminal
- Python 3
- ord()
- chr()
- Bitwise shift >>
- Bitwise AND &
- grep / cat

## Key Learning

- Two 8-bit characters can be combined into one 16-bit value.
- << 8 shifts a value left by 8 bits.
- >> 8 extracts the upper byte.
- & 0xff extracts the lower byte.
- ord() converts a character into an integer.
- chr() converts an integer back into a character.
- Reverse engineering often requires reversing the original encoding operation.

## Final Solution

Download enc

↓

Read the encoded characters

↓

Convert each character using ord()

↓

Extract the first byte using value >> 8

↓

Extract the second byte using value & 0xff

↓

Convert both values using chr()

↓

Join the characters

↓

Recover the flag

picoCTF{16_bits_inst34d_of_8_b7f62ca5}

## One-Line Explanation

The flag is recovered by reversing the 16-bit transformation with >> 8 and & 0xff.
