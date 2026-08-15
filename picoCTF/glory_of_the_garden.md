# Glory of the Garden
| **Platform** | picoCTF 2019 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | jedavis/Danny |
| **Topic** | JPEG / Hex Editor / Strings |

## Challenge

The challenge says:

```text
This file contains more than it seems.

Get the flag from garden.jpg.
The hint says:

What is a hex editor?

The challenge provides a JPEG file:

garden.jpg

The goal is to inspect the file and find hidden information inside it.

Step 1 – Download the File

Download garden.jpg from the challenge.

In the webshell, you can use:

wget <garden.jpg-download-link>

After downloading, check that the file exists:

ls -l
Step 2 – Inspect the File

A simple first step is to use the strings command:

strings garden.jpg

This extracts printable ASCII strings from the binary JPEG file.

Because the challenge is a Forensics challenge and the hint mentions a hex editor, hidden text may exist inside the image data.

Step 3 – Search for the Flag

Instead of manually reading all the output, search specifically for pico:

strings garden.jpg | grep -i "pico"

The output reveals:

Here is a flag: picoCTF{more_than_m33ts_the_3y3339cbe6dc}

Therefore, the hidden flag is:

picoCTF{more_than_m33ts_the_3y3339cbe6dc}
Why This Works

A JPEG file is a binary file, but binary files can still contain readable ASCII text.

The strings command scans the file and extracts sequences of printable characters.

The flag was embedded inside garden.jpg, so:

strings garden.jpg

can reveal it without needing to open the image in a normal image viewer.

Searching with:

strings garden.jpg | grep -i "pico"

makes the process much faster.

Hex Editor Concept

The hint asks:

What is a hex editor?

A hex editor allows you to view the raw bytes of a file.

For example, you could inspect the JPEG with:

xxd garden.jpg | less

or:

hexdump -C garden.jpg | less

Readable text embedded in the binary data can sometimes be spotted directly in the ASCII portion of the output.

However, for this challenge, strings is the quickest solution.

Commands Used

Download the file:

wget <garden.jpg-download-link>

Check the file:

ls -l garden.jpg

Extract readable strings:

strings garden.jpg

Search for the flag:

strings garden.jpg | grep -i "pico"

Optional hex inspection:

xxd garden.jpg | less
One-Line Solution
strings garden.jpg | grep -i "pico"
Complete Solution
Download garden.jpg
        ↓
Check the file
        ↓
Run strings garden.jpg
        ↓
Search for "pico"
        ↓
strings garden.jpg | grep -i "pico"
        ↓
Find the embedded flag
        ↓
Submit the flag
Flag
picoCTF{more_than_m33ts_the_3y3339cbe6dc}
Tools Used
Linux Terminal
wget
strings
grep
xxd
hexdump
Key Learning
Binary files can contain readable text.
strings extracts printable strings from binary files.
grep can quickly search command output.
A hex editor lets you inspect the raw bytes of a file.
Forensics challenges often hide information inside file data rather than the visible image.
Always inspect suspicious files with tools such as file, strings, and xxd.
Final Solution
garden.jpg
        ↓
strings garden.jpg
        ↓
Search for "pico"
        ↓
picoCTF{more_than_m33ts_the_3y3339cbe6dc}

