# Secret of the Polyglot
| **Platform** | picoCTF 2024 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | syreal |
| **Topic** | File Formats / Polyglot / PDF |

## Challenge

The challenge provides a suspicious file named:

    flag2of2

The hint says:

    This problem can be solved by just opening the file in different ways.

The important idea is that the file is a **polyglot**, meaning it contains data that can be interpreted as different file formats.

## Step 1 – Identify the File

Use:

    file flag2of2

The file contains PDF data, which can be identified by PDF structures such as:

    %PDF
    obj
    xref
    trailer
    %%EOF

## Step 2 – Follow the Hint

The hint tells us to open the file in different ways.

Instead of treating `flag2of2` as an ordinary file, open it using a PDF viewer.

The file successfully opens as a PDF.

## Step 3 – Find the Flag

The PDF contains the hidden flag.

The flag is:

    picoCTF{f1u3n7_1n_pn9_&_pdf_724b1287}

## Step 4 – Verify from the Terminal

We can also search the file for readable text:

    strings flag2of2 | grep -i "pico"

This reveals the flag.

We can also inspect the PDF structure:

    strings flag2of2 | grep -E "%PDF|obj|xref|trailer|EOF"

This confirms that PDF data is present inside the file.

## Why This Works

The challenge is based on a **polyglot file**.

A polyglot file can contain information that is valid or meaningful under multiple file-format interpretations.

The hint:

    This problem can be solved by just opening the file in different ways.

points us toward trying different applications and file viewers.

Opening `flag2of2` as a PDF reveals the hidden flag.

## Commands Used

Identify the file:

    file flag2of2

Search for the flag:

    strings flag2of2 | grep -i "pico"

Inspect PDF markers:

    strings flag2of2 | grep -E "%PDF|obj|xref|trailer|EOF"

## One-Line Solution

    Open flag2of2 as a PDF → find the embedded picoCTF flag.

## Complete Solution

    Download flag2of2
            ↓
    Run file flag2of2
            ↓
    Notice PDF data
            ↓
    Follow the hint
            ↓
    Open flag2of2 as a PDF
            ↓
    Read the hidden information
            ↓
    Find the flag
            ↓
    picoCTF{f1u3n7_1n_pn9_&_pdf_724b1287}

## Flag

    picoCTF{f1u3n7_1n_pn9_&_pdf_724b1287}

## Tools Used

- Linux Terminal
- `file`
- `strings`
- `grep`
- PDF Viewer

## Key Learning

- A file can contain data belonging to multiple formats.
- Polyglot files can behave differently depending on how they are opened.
- `file` helps identify suspicious file formats.
- `strings` can reveal readable information hidden inside binary files.
- Opening a suspicious file with different applications can reveal hidden data.
- Always follow the challenge hints when analyzing forensic files.
