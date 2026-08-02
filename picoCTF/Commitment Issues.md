# Commitment Issues

 **Platform**  picoCTF(Cylab)
 **Category**  General Skills 
 **Difficulty**  Easy 

## Challenge
The challenge provides a ZIP archive containing a Git repository.
The goal is to recover the flag by exploring the Git commit history and restoring an older commit.
The hints suggest using Git version control features, especially checking out previous commits.

## Approach
First, extract the downloaded ZIP file.
unzip challenge.zip
Move into the extracted directory.
cd drop-in
List the files to verify that it is a Git repository.
ls -la
The output shows a hidden `.git` directory, confirming that the folder is under Git version control.
Next, view the commit history.
git log --oneline
Example output:

87b85d7 create flag
68df363 remove flag
e247ddd initial commit
The commit named **create flag** looked interesting, so I checked it out.
git checkout 87b85d7
Git switched to that commit in a detached HEAD state.
After switching commits, I listed the files.
ls
A new file named `message.txt` appeared.
Finally, I displayed its contents.
cat message.txt
The file contained the flag.

## Terminal Output
$ unzip challenge.zip
$ cd drop-in
$ ls -la
.git
message.txt
$ git log --oneline
87b85d7 create flag
68df363 remove flag
e247ddd initial commit
$ git checkout 87b85d7
HEAD is now at 87b85d7 create flag
$ ls
message.txt
$ cat message.txt
picoCTF{s@n1t1z3_ea83ff2a}

## Flag
picoCTF{s@n1t1z3_ea83ff2a}

## Tools Used
- picoCTF WebShell
- Git
- Linux Terminal

## Key Learning
- Learned how to inspect Git commit history using `git log`.
- Used `git checkout` to move to an older commit.
- Understood how deleted files can still exist in previous commits.
- Learned that Git history can be used to recover lost or removed files.
