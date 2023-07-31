# GIT
Created: 2022-08-11 19:59
Tags: 
____


* made by Linus Torvalds(creator of Linux) in 2005

![[git0.png]]

* upload change whenever you want
* github is a site for hosting git repositories
* you can use git without github

#### Basic Concepts
* Untracked
	* Git ignores the file completely like it doesn't exist
* Unmodified
	* The file is tracked and it's not changes since the last commit
* Modified:
	* The file is changed but we are not sure we are going to commit it
* Staged:
	* Ready to commit and submit changes

![[git1.png]]


```bash

git status

git restore --staged 1.txt

git commit -m "something"

# Your branch is ahead of 'origin/main' by 1 commit.
# (use "git push" to publish your local commits)%%```

git push 

echo "second line" >> 1.txt

git status

#On branch main
#Your branch is up to date with 'origin/main'.
#Changes not staged for commit:
#  (use "git add <file>..." to update what will be committed)
#  (use "git restore <file>..." to discard changes in working directory)
#	modified:   1.txt
#no changes added to commit (use "git add" and/or "git commit -a")


git diff

git rm README.md
# On branch main
# Your branch is up to date with 'origin/main'.
# Changes to be committed:
# (use "git restore --staged <file>..." to unstage)
# deleted: README.md

# Changes not staged for commit:
# (use "git add <file>..." to update what will be committed)
# (use "git restore <file>..." to discard changes in working directory)
# modified: 1.txt

git log

git show 3b6aeba209933e0bc93b11fdb8f778a3a5511f93



git checkout 3b6aeba209933e0bc93b11fdb8f778a3a5511f93

```


![[git3.png]]


_____
##### References
1.

