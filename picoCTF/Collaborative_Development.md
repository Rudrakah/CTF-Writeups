# Collaborative Development
| **Platform** | picoCTF(Cylab) |
| **Category** | General Skills |
| **Difficulty** | Easy |

## Challenge
This challenge focuses on **Git collaboration**. The repository contains multiple branches, each contributing a different part of the flag. The goal is to merge all feature branches into the `main` branch while resolving any merge conflicts that occur. Once all branches are successfully merged, the completed `flag.py` file reveals the full flag.

## Approach
First, I extracted the challenge files and navigated into the repository.
```bash
cd collaborative/drop-in
```
I checked all available branches.
```bash
git branch -a
```
The repository contained multiple feature branches:
```text
feature/part-1
feature/part-2
feature/part-3
```
I configured Git with my username and email.
```bash
git config --global user.name "Rudraksh"
git config --global user.email "example@email.com"
```
Then I merged each branch into the main branch.
```bash
git merge feature/part-1
git merge feature/part-2
git merge feature/part-3
```
While merging, Git reported **merge conflicts** inside `flag.py`.
I opened the file to inspect the conflict markers.
```bash
cat flag.py
```
The file contained sections similar to:
```python
<<<<<<< HEAD
print("picoCTF{t3@mW0rk_", end='')
=======
print("m@k3s_th3_dr34m_", end='')
>>>>>>> feature/part-2
```
I edited the file using Nano to keep all required parts of the flag while removing the conflict markers.
```bash
nano flag.py
```
The final file combined all three flag fragments.
After saving the file, I staged the resolved file.
```bash
git add flag.py
```
Then I created a new commit.
```bash
git commit -m "Resolve merge conflict"
```
After merging all branches successfully, running the program displayed the complete flag.

## Commands Used
```bash
cd collaborative/drop-in
git branch -a
git config --global user.name "Rudraksh"
git config --global user.email "example@email.com"
git merge feature/part-1
git merge feature/part-2
git merge feature/part-3
cat flag.py
nano flag.py
git add flag.py
git commit -m "Resolve merge conflict"
python3 flag.py
```

## Flag
```text
picoCTF{t3@mW0rk_m@k3s_th3_dr3@m_w0rk_798f9981}
```

## Tools Used
- Git
- Linux Terminal
- Nano Editor
- Python 3

## Key Learning
- Learned how to list all Git branches using `git branch -a`.
- Practiced configuring Git username and email.
- Understood how to merge multiple feature branches into the main branch.
- Learned how to identify and resolve Git merge conflicts.
- Practiced editing conflicted files using Nano.
- Reinforced the complete Git workflow of merge → resolve conflicts → add → commit.
