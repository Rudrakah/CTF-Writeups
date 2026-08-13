# YaraRules0x100
| **Platform**   | picoCTF 2025              |
| **Category**   | General Skills            |
| **Difficulty** | Medium                    |
| **Author**     | Nandan Desai / syreal     |
| **Tags**       | browser_webshell_solvable |
| **Topic**      | YARA Rules                |

## Challenge
The challenge says:

```text
Seems like this file sneaked past our Intrusion Detection Systems, indicating a fresh threat with no matching signatures in our database.

Can you dive into this file and whip up some YARA rules? We need to make sure we catch this thing if it pops up again.
```

A suspicious archive is provided.

The challenge tells us to download the suspicious file and unzip it using the password:

```text
picoctf
```

After creating a YARA rule/signature, we need to submit the rule to the challenge server using:

```bash
socat -t60 - TCP:standard-pizzas.picoctf.net:64701 < sample.txt
```

The filename `sample.txt` can be replaced with whatever filename contains our YARA rule.

## Hints

### Hint 1

```text
The test cases will attempt to match your rule with various variations of this suspicious file, including a packed version, an unpacked version, slight modifications to the file while retaining functionality, etc.
```

This means the rule should not depend on only one exact file hash or an easily changed property.

### Hint 2

```text
Since this is a Windows executable file, some strings within this binary can be "wide" strings. Try declaring your string variables something like $str = "Some Text" wide ascii wherever necessary.
```

This tells us that YARA strings can be declared using:

```yara
wide ascii
```

to match both normal ASCII and UTF-16-style wide strings.

### Hint 3

```text
Your rule should also not generate any false positives (or false negatives). Refine your rule to perfection! One YARA file can have multiple rules! Maybe define one rule for Packed binary and another rule for Unpacked binary in the same rule file?
```

This means we need to create a robust YARA rule that detects the suspicious file in multiple forms while avoiding unrelated files.

## Step 1 – Inspect the Files

After starting the instance, inspect the available files:

```bash
ls
```

The directory contains several suspicious files and supporting files.

Useful commands for identifying file types include:

```bash
file suspicious.*
```

and:

```bash
ls -lh
```

We can also inspect strings inside suspicious files using:

```bash
strings suspicious.exe
```

or:

```bash
strings suspicious.exe | less
```

## Step 2 – Extract the Suspicious Archive

The challenge provides a ZIP archive protected with the password:

```text
picoctf
```

Extract it with:

```bash
unzip suspicious.zip
```

When prompted for the password, enter:

```text
picoctf
```

Then inspect the extracted files:

```bash
ls
```

## Step 3 – Identify the Windows Executable

The suspicious file is a Windows executable.

Use:

```bash
file suspicious.exe
```

A Windows executable will normally be identified as a PE/PE32 executable.

This is important because the challenge hints that some strings may be stored as wide strings.

## Step 4 – Examine Strings

Use:

```bash
strings suspicious.exe
```

For more detailed inspection:

```bash
strings -a suspicious.exe
```

and:

```bash
strings -el suspicious.exe
```

The goal is to identify strings or byte patterns that are characteristic of the suspicious file.

We should avoid using a hash as the main detection method because the challenge explicitly tests modified versions of the file.

## Step 5 – Create the YARA Rule

Create a rule file:

```bash
nano sample.txt
```

A basic YARA rule has this structure:

```yara
rule suspicious_file
{
    strings:
        $str1 = "suspicious_string" ascii wide
        $str2 = "another_string" ascii wide

    condition:
        uint16(0) == 0x5A4D and
        2 of ($str*)
}
```

The important sections are:

```text
strings:
```

and:

```text
condition:
```

The `uint16(0) == 0x5A4D` check identifies a Windows PE file because `MZ` is the standard PE/DOS header.

## Step 6 – Use ASCII and Wide Strings

Because the challenge specifically warns about wide strings, strings can be declared as:

```yara
$str = "Some Text" ascii wide
```

This allows the rule to match both normal ASCII and wide-character representations.

For example:

```yara
strings:
    $s1 = "cmd.exe" ascii wide
    $s2 = "powershell" ascii wide
```

This makes the rule more resistant to different string representations.

## Step 7 – Avoid Hash-Based Detection

A weak rule would be something like:

```yara
rule bad_rule
{
    condition:
        hash.md5(0, filesize) == "..."
}
```

This would fail against modified versions of the executable.

The challenge specifically tests:

```text
Packed version
Unpacked version
Slightly modified versions
```

Therefore, we should detect characteristics of the malware rather than its exact hash.

## Step 8 – Multiple YARA Rules

The challenge allows multiple rules inside one YARA file.

For example:

```yara
rule Packed_Binary
{
    strings:
        $s1 = "characteristic_string_1" ascii wide
        $s2 = "characteristic_string_2" ascii wide

    condition:
        uint16(0) == 0x5A4D and
        2 of ($s*)
}

rule Unpacked_Binary
{
    strings:
        $s1 = "characteristic_string_3" ascii wide
        $s2 = "characteristic_string_4" ascii wide

    condition:
        uint16(0) == 0x5A4D and
        2 of ($s*)
}
```

The exact strings should be chosen after inspecting the suspicious executable.

## Step 9 – Save the Rule

In `nano`:

```text
Ctrl + X
```

Then:

```text
Y
```

and press:

```text
Enter
```

Verify the file:

```bash
cat sample.txt
```

## Step 10 – Submit the YARA Rule

The challenge provides the submission command:

```bash
socat -t60 - TCP:standard-pizzas.picoctf.net:64701 < sample.txt
```

Run it from the Webshell.

The server tests the submitted YARA rule against multiple versions of the suspicious file.

In the solved instance, the terminal returned:

```text
Status: Passed
Congrats! Here is your flag:
```

## Step 11 – Get the Flag

The server returned:

```text
picoCTF{yara_ruI35_r0ckzzz_714786c1}
```

The challenge page also showed:

```text
Correct flag!
```

## Commands Used

List files:

```bash
ls
```

Identify file type:

```bash
file suspicious.*
```

Extract the archive:

```bash
unzip suspicious.zip
```

Inspect strings:

```bash
strings suspicious.exe
```

Open the YARA rule:

```bash
nano sample.txt
```

Display the rule:

```bash
cat sample.txt
```

Submit the rule:

```bash
socat -t60 - TCP:standard-pizzas.picoctf.net:64701 < sample.txt
```

## Important Concepts

### YARA

YARA is a pattern-matching tool commonly used to identify malware and suspicious files.

A YARA rule generally contains:

```text
rule
strings
condition
```

### ASCII vs Wide Strings

Normal ASCII:

```yara
$str = "hello" ascii
```

ASCII + wide:

```yara
$str = "hello" ascii wide
```

Using `ascii wide` is useful when dealing with Windows executables.

### PE Header

Windows executables generally start with the `MZ` signature:

```text
0x5A4D
```

In YARA:

```yara
uint16(0) == 0x5A4D
```

can be used to verify that the file is a Windows PE-style executable.

### Robust Detection

A good malware signature should detect characteristics that remain present even when:

* The file is packed.
* The file is unpacked.
* Small modifications are made.
* Some non-essential bytes change.

Therefore, relying on unique strings, PE characteristics, and multiple conditions is better than relying on a single hash.

## Key Learning

* Learn how YARA rules are structured.
* Use `strings` to investigate suspicious executables.
* Understand ASCII and wide-character strings.
* Use multiple YARA rules when different variants need different detection logic.
* Avoid relying only on hashes for malware detection.
* Use PE headers and characteristic strings to create more robust signatures.
* YARA can be used for malware detection and threat hunting.

## Final Solution

```text
Start the instance
        ↓
Download suspicious.zip
        ↓
Unzip using password: picoctf
        ↓
Inspect the suspicious executable
        ↓
Use file / strings to investigate it
        ↓
Identify characteristic strings/patterns
        ↓
Create a YARA rule
        ↓
Use ascii wide where necessary
        ↓
Save the rule as sample.txt
        ↓
Submit using socat
        ↓
Server tests the rule
        ↓
Status: Passed
        ↓
Flag
```

## Flag

```text
picoCTF{yara_ruI35_r0ckzzz_714786c1}
```
