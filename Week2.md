# Elavator Challenge Week 2
## Matthew Gilligan

## Recap
This was my second ever CTF! Even though I had never used Linux before, I was able to find the flag and learn a few commands along the way! The meet started out with us talking about lots of new Linux commands and getting some fun stickers. We were introduced to 8 new commands that we could use in linux, and were encouraged to look up other commands that might make it easier to find the flag. The commands that we learned were:

pwd, cat, cd, ls, grep, ssh, su, and man.

pwd stands for print working directory, and when entered will output the absolute path of the current directory that you're in. It is mostly used to confirm your current location in the directory structure. We used pwd in this exercise to confirm where we were. If I were to explain it in with the analogy of an elavator, this is the sign that says "you are on level _".

cat stands for concatenate, and when entered before the name of a file, will output the contents of that file. There are many options that you can add-on to cat, like -n to number all output lines or -b to number all blank output lines, but during this challenge I didn't use any add-ons. I used cat very frequently in this exercise, as there were many files that I needed to read. In the context of the elavator, cat is the box opener that allows you to find the information inside the files. 

cd stands for change directory, and when entered along with the directory that you want to change to, changes the working directory into the one that you specified. We learned that you can use cd ~ to go to the home directory, or cd .. to move "up"/"back" a directory. We used cd to change directories so that we could read different files and uncover more information. 

ls, standing for list, will list all files and directories in the current directory when entered. It was one of the most common commands that I used this challenge, as I needed to understand what was actually inside the level that I was in. When thinking about the elavator analogy, ls is sort of like the "flashlight" that highlights everything in the directory that you're in and all the places where information is available.

grep, global regular expression print, is a search command that searches all the text within files. After entering grep, followed by "something" and the filename + ".txt", grep will return all matching lines with "something" in filename.txt. You can add on a lot of functionality to grep, such as -r which searches recursively in all files within the directory, as well as all of its subdirectories. There are many other add-ons, but -r was the main one that I used. grep is very useful when the files are too big to manually search, or there are too many files to search through. In the elavator analogy, grep is like a magnet that you can tune to attract a specific thing. 

ssh stands for secure shell and is a way to connect to remote servers over a network. This is the way that we were able to access the challenge. We used the -p add-on in order to specify the port that we were accessing. This is like the elavator door, or the door to the lobby that leads to the elavator. This is a common start point.

su stands for switch user, and it switches to a specified user account. This is a common means of hacking, and usually it requires a password. There are definetely means of switching without a password, but those are more advanced that what I currently know. su is kinda like hitting the button to the next floor on the elavator.

man stands for manual, and allows for more complex commands. We used man to add extra commands to grep and other commands. I still don't fully understand man, but I do realize that it is complicated and allows for a lot more user control. 

Overall, these were the commands that we learned at meet 2, and were all of the commands that we needed to solve this challenge. 
## Overview
After using "ssh chal@elevator.ctf-league.osusec.org -p 1302" to access the challenge, we were tasked with hacking 7 accounts, each with a different way of accessing their information. We needed to use linux commands in order to get the name and password of the account, which would allow us to switch user in order to access different information. This "segmented" approach was made to resemble elavator floors. This way of separating information and uncovering more and more information through different users is a common offensive security practice.
## Challenge of the week
For the first level, we typed "imready" after authenticating to the shell. We found a directory called "creds_level0". After some initial struggle with my first ever linux commands and accidently changed and read the directory "above" home, I found a README file, which contained the username and password for level_1. With a "su --login 'username' ", and entering the password, we were able to move up a floor into level 1. 

In level 1, I used a ls command to reveal 2 directories and a README. The README stated "99% of gamblers quit right before they find the creds", then told me to use grep to "run around directories like you're a little monkey running around trees". After initially reading and ls'ing the directories, I found more nested directories. While I knew that the hint told me to use grep, it wasn't until my teammate told me that grep could search through all the directories that I tried using it. I tried "man grep" which gave me a very long list of all the things that grep could do. I then tried "grep -r level" which searched all directories recursively and returned everything with "level" in it. This returned "level2_14110", so I then did "grep -r password" to find "8e5bf0". This allowed me to switch user to get to the next level. 

In level 2, with my new knowledge of grep, I repeated the same process that I used in level 1. I used ls to find a README and creds_level3.txt. I once again used cat to read the README, which told me "rocky racoon checked in his room..." and also told me to use grep again. However, I accidently used "grep level" instead of "grep -r level", which took me to text input, where it would highlight level in the text that I inputted. I needed to use "^C" to end the grep command so that I could re-enter "grep -r level" and "grep -r password". ^C will terminate a running process in the terminal and it is very useful to have on hand in case anything goes wrong. After finding the level name and password, I switched user to get to level 3.
In level 3 
## Reflection
 I'm very grateful for all the support from my team, the coaches and the team next to me. 
