# MY GIT
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
The challenge provides access to a custom Git server. The objective is to clone the remote repository, follow the instructions in the repository's `README`, create and commit a flag file, and push the changes back to the remote server.
The server validates the Git commit and, if all requirements are satisfied, returns the flag.

## Approach
First, I cloned the repository from the remote Git server.

```bash
git clone ssh://git@foggy-cliff.picoctf.net:54431/git/challenge.git
```
After entering the provided password, I navigated into the cloned repository.
```bash
cd challenge
```
I read the instructions inside the repository.
```bash
cat README.md
```
The README explained that I needed to create a file named `flag.txt`, commit it using my Git identity, and push the commit back to the remote repository.
I created the required file.
```bash
touch flag.txt
```
Then I staged the file.
```bash
git add flag.txt
```
Next, I created a commit.
```bash
git commit -m "Add flag"
```
Finally, I pushed the commit to the remote server.
```bash
git push origin master
```
The server verified the commit author and detected the required `flag.txt` file. After successful validation, it returned the flag.

## Commands Used
```bash
git clone ssh://git@foggy-cliff.picoctf.net:54431/git/challenge.git
cd challenge
cat README.md
touch flag.txt
git add flag.txt
git commit -m "Add flag"
git push origin master
```

## Flag
```text
picoCTF{1mp3rs0n4t4_gi7_345ye522152d}
```

## Tools Used
- Git
- Linux Terminal
- SSH

## Key Learning
- Learned how to clone a remote Git repository.
- Practiced reading repository instructions from a README file.
- Understood the Git workflow of **add → commit → push**.
- Learned how remote Git servers can validate commits based on author information and repository contents.
