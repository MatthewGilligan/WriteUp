# Week 6
## Matthew Gilligan

## Recap
This week we learned about Git, and how to use it to navigate through files, undo, load previous states, create repos/branches, and see all changes made to a directory or repository. 

We began by talking about how linux/ unix systems are very good at deleting and creating things, and there is no undoing them. They will even allow you to brick your computer if you want to and know how to. We then talked about how about how Git is complimentary to these systems, as it can be used to save progress and create "branches" of a project that each do different things.  

### Demo
At the start of the class we saw a demo.

The demo began with creating a repo (repository) with "git init"
The next command was "echo "hey everybody" >> hey" , this outputs "hey everybody" then appends the output to the new file called "hey". 
We then used "git status" to show that we were on teh main branch, and that we had an untracked file. (Untracked files in the working directory that git hasn't been instructed to keep track of. They are unstaged and uncommitted. You can add them using "git add" )

This was it for the demo, but we also went over some useful commands and terms in git.

### Git commands and terms
Some of the useful commands and terms we learned were: 

#### Commit: 
A commit is an entry into git history. Use git commit -m"Your commit title here" to commit

#### Git Status
This command shows you if there is anything left to commit, it also gives a a brief status, and gives you your status. 

#### Git log
This command tells you your commit history with the names of your commits. 

#### Git Branches
Git uses branches that split off from each other, that can be worked on without affecting the other. 

#### Git reflog
This was marked as the "Most Important" command that we learned. This command keeps track of literally everything that has happened, and is insane for debugging. It will show you previous states, so you can use other commands to "rewind time" to those states. 

#### Git HEADs
In git, a HEAD is a reference to the most recent commit on the current branch. It is like a linked list, where the HEADs are "on top" of each other. 
There is a state called a "detached HEAD state" where any commits aren't tied to a specific branch. This state occurs when the HEAD points directly to a specific commit instead of to a branch. This is very useful for examining past commits and making temporary changes. 
When you switch branches, HEAD moves to point to the latest commit on that specific branch. 
HEADs can be used to restore to the previous states because they point to specific commits. 

#### Git checkout 
The git checkout command is used to switch between branches, allowing for navigation through a project's history. You can use the git checkout command by typing "git checkout "branchName" " 
You can also use checkout to restore files to a specific state after a commit. 
Using git checkout to a specific commit is how you put yourself into a detached HEAD state. 
Two alternatives for git checkout are git switch (which switches between branches) and git restore (which restores working tree files)



## Overview
Learned about git

## Challenge of the week
We used git

## Reflction
I enjoyed learning about git
