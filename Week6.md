# Week 6
## Matthew Gilligan

## Recap
This week we learned about Git, and how to use it to navigate through files, undo, load previous states, create repos/branches, and see all changes made to a directory or repository. I've used gitHub, and gitKraken before, so I knew the basic applications of git (to hold and merge information), but I didn't know any of the actual language of git. 

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

Based off of this message, I knew that I wanted add checking for more branches into my search process. I started by once again ls'ing and actually found a file called credsLevel2.txt, however this file was faking me out, because the next user and password were both equal to "??????".
After this I used git reflog, which revealed 4 changes:
"0ba6ce9 (HEAD -> master) HEAD@{0}: checkout: moving from dontlookinhere to master
ef4d6a5 (dontlookinhere) HEAD@{1}: commit: next time it won't be this easy 
0ba6ce9 (HEAD -> master) HEAD@{2}: checkout: moving from master to dontlookinhere
0ba6ce9 (HEAD -> master) HEAD@{3}: commit (initial): TODO store credentials"

The changing of branches indicates a second branch called "dontlookinhere". I then used the git branch command to verify, which confirmed that there were two branches. I used the git switch command "git switch dontlookinhere" in order to switch branches to the other branch. When I used ls in this branch, I found another file called credsLevel2.txt, which contained the question marks, but also contained the username and password for level 4. 

### Level 4
Level 4's README said: " Swipa's escalated their tactics...there's a bunch of fake passwords added in. Can you figure out which line wasn't created by Swipa?" 
After finding the creds file, I opened it with cat, and found hundreds of usernames and passwords, just like the README told me. I remembered that we could use the git blame command in order to track who edited what line. I entered "git blame credsLevel2.txt", and it just kept on generating more and more until I quit using ctrl + c to stop the generation. 
This level stumped me for a while, because I kept trying to find a way to comb through all of the usernames and passwords with commands. I kept looking things up, but I couldn't find a solution that didn't involve generating the whole data set first. I then decided to just generate the whole data set and comb through it while I was generating it. I kept holding the "enter" key, and eventually stopped when I saw "xX_liltimmyT_Xx" as the author of the username and password, level5_1965, 062930. I entered these into su and switched to level 5.
(Later, I found out that I could've used git log "log.txt" to put all of the entries into a txt file and use a regular expression (regex) that searches through the file. I do not currently have a solid grasp on regexes, but I definitely want to learn more, because they seem really useful)

### Level 5
This level was solved fairly quickly by ls'ing and finding a file called "credsLevel6.txt". I then cat'd this file and it contained the user name and password for level 6. 

### Level 6
After cd'ing into the repository, I found a file called "secretPlan.txt". Inside this file, I found the text: 
"I'll make a secret branch...
and add my creds to creds_level7.txt
and then commit that file...
and then delete that file...
and then delete the branch...
and then forget to clear something very important...
it will be impossible to find my creds...
perfect security achieved. no thanks to swipa"

I got distracted by the part about a secret branch, and used the git branch command, only to see that there was only one master branch in this repository. I then even used "git checkout master" in hopes that master was somehow the secret branch. I then remembered the part about the deltion of the branch, I checked git reflog in order to see when the branch was deleted and checkout to when the branch existed. Git reflog revealed: 
"fa1fdc7 (HEAD -> master) HEAD@{0}: checkout: moving from fa1fdc7601b6510633e700ffcad5ac488e95ba43 to master
fa1fdc7 (HEAD -> master) HEAD@{1}: checkout: moving from master to fa1fdc7
fa1fdc7 (HEAD -> master) HEAD@{2}: checkout: moving from supersecretcredentialbranch to master
d6ec4d6 HEAD@{3}: commit: deleted! time to delete the branch now...
67cdd87 HEAD@{4}: commit: stored my credentials! now I'll delete them
fa1fdc7 (HEAD -> master) HEAD@{5}: checkout: moving from master to supersecretcredentialbranch
fa1fdc7 (HEAD -> master) HEAD@{6}: commit (initial): secret plan phase 1 - planning "

I saw where the branch had been deleted, so I used git checkout to 67cdd87, the commit where they "stored my credentials! now I'll delete them". I then used ls to reveal a new file called credsLevel7.txt. I opend this file and it revealed the user name and password for level 7. 

### Level 7
Level 7's README said: 
"Sigh... I guess I'll stop using git for my credentials :(
...
...
...
I'll keep them in in /flag7.txt!" 

When I used ls, I only saw README, so I tried ls -a, thinking that it would reveal flag7.txt, however, it revealed 3 other files. These were ".bash_logout", ".bashrc" and ".profile". cat'ing .bash_logout just said that it was executed when the login shell ends, and that it clears the screen. cat'ing .profile gave information regarding setting PATH so it would include a user's private bin, given that the bin exists. I then tried to use git status, but I wasn't in a repository so this didn't work. I thought that I would need to create or clone a repository and then search through it, so I used git init to create a new repository. I then used git reflog to confirm that it was new (which it was. It said: fatal: your current branch 'master' does not have any commits yet). I then tried entering "/flag7.txt", but was told "-bash: /flag7.txt Permission Denied". I then used ls and found a gigantic list of all of the levels that were used in this challenge. I re-entered the user name and password for level_7_1965, which was the one that I got from level 6. After getting back to what was essentially the start of level 7 again, I tried to just cat /flag7.txt even though I didn't see it listed with ls. This actually worked, and I got the flag to submit for points. 

##### Why could I read that file without seeing it?
I was very confused as to why I could read this file without seeing it listed under ls, even after using ls -a, which is supposed to reveal all hidden files. I asked copilot and here were the reasons that it thought could be it. 
1) Hidden files
2) Different directory listing
3) File permissions
4) Symbolic links
5) Wildcards or patterns

I don't that that is was 1 or 2, as I already checked the hidden files, and there were no other directories. It could've been 3, as I didn't check the permissions of the file. It also could be 5, as maybe something overlooked it, but I doubt it.
I didn't understand what 4 was, so I asked it to explain, and it told me that symbolic links are special types of files that point to another file or directory. 
They act as shortcuts, allowing us to access the target file using a symbolic link. If /flag7.txt was a symbolic link, it might not be listed when using ls due to directory permissions. Also, using cat on /flag7.txt would follow the symbolic link and display the contents of the target file, even if the symbolic link wasn't displayed by ls. Allow these factors lead me to belive that /flag7.txt was a symbolic link, making number 4 the reason I couldn't see it, but still read it. 

## Reflction
I enjoyed this lesson in git forensics, and I am happy that I finished this challenge. I had never used the actual git language before (only gitHub and gitKraken), so I was very excited to learn more. I actually really enjoyed learning about git and using it in combination with linux commands, but I think that I'll keep doing these writeups in gitHub, not the actual git. Overall it was another fun and informative week!

