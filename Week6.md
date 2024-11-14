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

#### Git clone
This command clones an existing repository.

#### Git add 
This command is how you actually stage your change to be committed (git add <file-path>)

#### Git blame
This command displays the author of the changes to each line of a file


## Overview
For this week's challenge "git_forensics", we were tasked with recovering credentials that were hidden in git repositories. I used powershell, but it was completable with linux or any other way to ssh. This challenge was very similar to the elavator challenge from week 1, where there were multiple levels to "solve" before finding the flag. The main difference was that instead of only using linux commands, we also incorporated git commands to switch between repositories. 

At the start of the challenge, there was a list of "useful commands" put up on the board. These commands were:

git status
git log
git reflog
git checkout <hash>
get checkout <name of branch>
git blame <file>


## Challenge of the week
After ssh'ing in and ls'ing, we found a file that said: "Thank god you're here!! I had my credentials stored in my_repository but somebody deleted them! can you cd to my_repository and help me restore them?" 

### Level 0
I used cd to change into the new repository and found a lot of files. These files were branches (a direcotry), config, description, hooks (a directory), index, info (directory), logs (directory), objects (also a directory), and refs (also a directory). I cat'd the non-directory files, but didn't make much progress. The files all either said the titles of commits, or text that looked like: "DIRCg)��x��g)��x���*����(��{�
                             B�{m�<wx�F2�creds_level1.txtTREE1 0
q�."

While I'm sure that this is useful, at my current level of understanding, I could not figure out how to use this. 

I then realize that I needed to use the git commands in order to track the commits. I used git reflog, which revealed: "aa67c16 (HEAD -> master) HEAD@{0}: commit (initial): stored my credentials! nice and safe :)" The important part of this was the initial part, "aa67c16". I used git checkout a67c16, which revealed "creds_level1.txt" as well as put me in a detached HEAD space. I then cat'd the creds file, revealing the next user and next password, which I then switched into using the su command. 

### Level 1 
At the start of level 1, I used ls to find a README, which said: "Thanks! But it looks like he got back in and did it again... Help get my creds!" and also showed that there was repository called my_repository. I once again cd'd into my_repository, and used ls-a to show that there was a git repository. I used git status which said that I was on the master branch with a clean working tree (nothing to commit). Since I had verified that there were no hidden files, I then used git log to check the history. This showed me: Author: imacybercriminal <yungswipa@ilovecrime.com>
Date:   Tue Nov 5 02:33:08 2024 +0000

    a84c6f is a terrible password for level2_1965... hehehee... catch me if you can!

commit 1111f6f782876d5054767310be785ee1516ee36f
Author: xX_liltimmyT_Xx <littletimmy@roblox.com>
Date:   Tue Nov 5 02:33:08 2024 +0000

    stored my credentials! nice and safe :)"

Apparently our cybercriminal is called yungswipa, and they just told us the username and password for level 2. I su'd into level2_1965 with a84c6f as the password, and it took me to the next level. 

### Level 2
The README for this level stated: "Wow, insulting my password was a low blow...But this time, they didn't leave any taunting messages. Go check out the situation in my repository and see if you can repair things...". Following the advice of the README, I cd'd into my_repository and once again used git status and git reflog. This revealed two lines: 608050d (HEAD -> master) HEAD@{0}: commit: hehe, catch me if you can!
06ff195 HEAD@{1}: commit (initial): stored my credentials! nice and safe :)

I assumed that I needed to use git checkout into one of these heads. I first tried the 608050d "hehe, catch me if you can" HEAD, but this didn't contain the creds. I used the git reflog command again (where I actually saw a new entry called "moving from master to 608050d" which was pretty cool to see that my progress was tracked", and then used git cheeckout to the 06ff195 HEAD. In this HEAD I found "creds_level2.txt", which when cat'd showed me the username and password for level 3. 

### Level 3
This README stated: 
Holy moly! We've been hit by yung swipa!
He's a cybercriminal and he loves crime!
We're up against an international playa!
Woah! All these lines line up perfectly!
I didn't get around to storing my creds!
Maybe check for some more branches here?

Based off of this message, I knew that I wanted add checking for more branches into my search process. I started by once again ls'ing and actually found a file called credsLevel3.txt, however this file was faking me out, because the next user and password were both equal to "??????". I then used 



## Reflction
I enjoyed learning about git
