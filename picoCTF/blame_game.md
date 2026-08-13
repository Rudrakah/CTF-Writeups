
# Blame Game
| **Platform** | picoCTF 2024 |
| **Category** | General Skills |
| **Difficulty** | Easy |
| **Author** | Jeffery John |
| **Topic** | Git / Git Blame |

## Challenge

The challenge says:

```text
Someone's commits seems to be preventing the program from working. Who is it?

You can download the challenge files here:
challenge.zip
```

The main idea is to inspect the Git history and determine which user made the problematic change.

## Step 1 – Extract the Challenge

Download `challenge.zip` and extract it:

```bash
unzip challenge.zip
```

Enter the extracted challenge directory:

```bash
cd challenge
```

Check the files:

```bash
ls -la
```

Because this challenge involves Git, look for the `.git` directory:

```bash
ls -la
```

If `.git` exists, the repository history is available.

## Step 2 – Check Git History

The first useful command is:

```bash
git log
```

This displays the commit history.

You can also use the shorter format:

```bash
git log --oneline
```

The challenge asks:

```text
Someone's commits seems to be preventing the program from working. Who is it?
```

So we need to identify the commit responsible for the problematic line.

## Step 3 – Inspect Changes

Use:

```bash
git log -p
```

This displays the commits together with the changes introduced by each commit.

Look for a commit that modified the Python program in a way that caused it to stop working.

Another useful command is:

```bash
git diff
```

This compares changes between versions.

## Step 4 – Use Git Blame

The first hint says:

```text
In collaborative projects, many users can make many changes. How can you see the changes within one file?
```

The command designed for this is:

```bash
git blame <filename>
```

First identify the Python file:

```bash
find . -type f
```

Then run:

```bash
git blame <filename>
```

For example, if the program is called `message.py`:

```bash
git blame message.py
```

Git blame shows which commit and author last modified each line.

The output generally looks like:

```text
commit_hash (Author Name date) line
```

This allows us to identify the person responsible for the problematic line.

## Step 5 – Identify the Author

The important information is the author associated with the commit that introduced the broken code.

You can inspect the commit in more detail using:

```bash
git show <commit_hash>
```

For example:

```bash
git show abc1234
```

This displays:

- Commit ID
- Author
- Date
- Commit message
- Changes introduced by the commit

The author of the problematic commit is the person the challenge is asking for.

## Step 6 – Get the Flag

The flag obtained from the challenge is:

```text
picoCTF{@sk_th3_1nt3rn_cfca95b2}
```

## Why This Works

Git keeps a complete history of changes made to files.

The important command for this challenge is:

```bash
git blame
```

`git blame` associates every line of a file with the commit and author that last changed that line.

Therefore, if a particular line is responsible for breaking the program, Git blame can tell us who introduced that line.

The general workflow is:

```text
Extract challenge.zip
        ↓
Enter the Git repository
        ↓
Inspect Git history
        ↓
Find the Python source file
        ↓
Use git blame
        ↓
Find the suspicious line
        ↓
Identify its commit
        ↓
Use git show
        ↓
Find the author
        ↓
Get the flag
```

## Important Git Commands

### View Commit History

```bash
git log
```

### Compact Commit History

```bash
git log --oneline
```

### View Commit Changes

```bash
git log -p
```

### Find Who Changed Each Line

```bash
git blame <filename>
```

### Inspect a Specific Commit

```bash
git show <commit_hash>
```

### View Repository Status

```bash
git status
```

## Key Learning

- Git stores the history of changes made to files.
- `git log` displays commit history.
- `git show` displays the details of a particular commit.
- `git blame` identifies the commit and author responsible for each line.
- Git history can be extremely useful when debugging broken code.
- In collaborative projects, `git blame` helps determine who last modified a specific line.

## Final Solution

```text
Download challenge.zip
        ↓
unzip challenge.zip
        ↓
cd challenge
        ↓
Check the Git repository
        ↓
git log
        ↓
Find the source file
        ↓
git blame <filename>
        ↓
Locate the suspicious line
        ↓
Identify the commit hash
        ↓
git show <commit_hash>
        ↓
Identify the author
        ↓
Submit the flag
```

## Flag

```text
picoCTF{@sk_th3_1nt3rn_cfca95b2}
```

## Tools Used

- Linux Terminal
- Git
- `git log`
- `git blame`
- `git show`
- `git diff`
- ZIP extraction

## One-Line Useful Commands

```bash
unzip challenge.zip && cd challenge && git log --oneline
```

```bash
git blame <filename>
```

```bash
git show <commit_hash>
```
