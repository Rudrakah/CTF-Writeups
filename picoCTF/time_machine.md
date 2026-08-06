# Time Machine
| **Platform** | picoCTF 2024 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
This challenge provides a Git repository and asks us to recover the flag. The hints indicate that simply reading the files is not enough and suggest looking at the Git commit history.

## Files Provided
```text
challenge.zip
```

## Approach
First, I extracted the downloaded archive and navigated into the challenge directory.
```bash
unzip challenge.zip
cd drop-in
```
Since the hints pointed toward Git history, I inspected the commit log.
```bash
git log
```
The repository contained a commit that looked interesting. To inspect what was committed, I displayed the details of that commit.
```bash
git show
```
The commit information revealed the flag directly inside the commit message/history.

## Commands Used
Extract the challenge files:
```bash
unzip challenge.zip
```

Navigate to the repository:
```bash
cd drop-in
```

View the commit history:
```bash
git log
```

Display the latest commit and its contents:
```bash
git show
```

## Explanation
- `unzip challenge.zip` extracts the challenge repository.
- `cd drop-in` enters the Git repository.
- `git log` lists the repository's commit history.
- `git show` displays the selected commit, including its metadata, commit message, and file changes. The flag was present in the commit information.

## Flag
```text
picoCTF{t1m3m@ch1n3_b476ca06}
```

## Tools Used
- Git
- Linux Terminal
- `git log`
- `git show`

## Key Learning
- Learned how to inspect a repository's commit history using `git log`.
- Practiced viewing commit details with `git show`.
- Understood that sensitive information may remain accessible in Git history even after files are modified or removed.
- Reinforced the importance of checking commit history during Git-based CTF challenges.
