# Week 3 challenge: super_Memory
## Matthew Gilligan


## Recap

This week we learned a lot about malware and even how to hack the hacker back. This was super cool and new to me, as I had never done this before, and I also got stuck on this during the Deadface CTF event. We learned specifically about C# Malware analysis, where you find the code and examine what is does to the computer. 
### C# malware analysis involves 
1) collection: Gathering the malware/ related data (logs or network traffic)
2) Static analysis: Examining the malware without executing it. This involves the use of tools (we used dnSpy, but I'll get to that later)
3) Dynamic analysis: Running the malware in a controlled environment and observing the behavior. (I installed a virtual machine called VMware)
4) Code analysis: DECOMPILING (This was the main thing we did this week). Decompiling allows you to see the source code and analyze the logic behind it. It goes from the code being told to the machine back into "normal" code.
5) Behavior analysis: Monitor the malware in real time and observe how it interacts with the network and system. (Helpful tools I've yet to install: Wireshark)
6) Reverse engineering: Basically remake the malware, and break it into little parts to better understand how everything interacts.

### Decompiling and debugging
Decompiling and debugging was the main thing that we were doing this week. In order to decompile, you need tools that you can install onto your computer. For this week, we used a tool called dnSpy, which takes code and transforms it back into normal code. The reason that we are using C#, is because it uses bytecocde as opposed to machine code. Machine code is the thing that goes directly into the computer, like assembly, while bytecode needs to go through another interpreter before being transformed into machine code. It is a "higher level" language that has more safety nets and features, but might not be as fast. 

After we had our dnSpy installed, we needed to drag a .dll file into it. A dll file stands for dynamic link library, which is like reusable code. Similar to a real life library, dll's "borrow" stuff without duplicating them. They borrow functions and resources and can be shared across multiple programs like a storing space. 

We installed dnSpy, a decompiling and debugging software that uses the dll files in order to read and break apart code. 

We then went through a brief overview of the main concepts that we would be using in the dnSpy software. We learned how to set a breakpoint, examine code, and do a basic check for vulnerabilities. Breakpoints stop the program at a certain line and show everything that is at play so that you can accurately analzye the code and what it's affecting. 

### Malware

We then were briefed on types of malware and mostly learned about information stealer malware, which is the type that we dealt with for this challenge. Info stealer malware looks through your computer for any valuable information, such as credit card, credentials, crypto wallets, important ssh keys, and it can attack anywhere on your computer. One example of an info stealer malware is redline stealer. Redline stealer collects various information, credentials and locates any firewalls on your computer. 

All malware is run through a Command and control center, often times called a C2 or C&C. This is how the hackers control the malware, and where you can attack them back. Often times, malware is shoddily made by amateur hackers and usually has some forms of typos or logic vulnerabilities.

### New(ish) acronyms

We also learned some new (not all new) acronyms that are commonly used and that all cybersecurity participants should know. 

FTP/SFTP: File transfer protocol/Secure file transfer protocol. 
This is how you move your files from your computer all over the internet. You upload the files to a server first. FTP uses two channels, control and data. The control channel manages the commands and responses and the data channel does the actual file transfers. 
The main difference between these is that SFTP uses ssh (secure shell) for encrption and is more secure. 

HTTP: HyperText Transfer Protocol: This protocol is used for transmitting hypertexts (texts that link to other texts) that uses a request-response protocol. This means that clients (ex: web browsers) will send HTTP requests to servers (ex: websites), which then return responses containing the requested data. There are different types of requests, but the most common ones are GET (gets a website/resource/anything), POST (send data), and DELETE (delete something). 

API: Application programming Interface: This allows for programs to "talk" to each other and are generally human readable. They are unencrypted and are what websites use. 


## Overview
For this challenge, we needed to use the dnSpy software that we installed in order to accesss and solve the weekly challenges. There were 4 flags this week, each with a new skill tied behind them. We were given 2 files, with 2 challenges behind each file. 

The overaching theme of this week was malware. A kid called "timmy" was trying to download more RAM and found a file called _not_malware.exe and wanted us to check the file. We downloaded the file and used dnSpy to decompile and debug it. 

## Challenge of the week
When we debugged the file, we found a large string that was very long and ended in []. I didn't know what to do with this at all, but one of my teammates immediately recognized it as a base 64 file. We looked up a base 64 decoder website and plugged it in there to find the first flag. 

The next flag was in the sftp, which we again located with dnSpy. After we debugged it we found a port that we could sign in to. After we signed in there was a flag displayed, which we entered for the second flag. 

For the third flag, we downloaded another file, which was supposed to be a "free robux" link, but was actually information stealing malware. We debugged it and used the breakpoint after seeing a prompt for username, password and "secret code". We put the breakpoint in and it stoped after the line that used "secret code", revealing it to be a flag. 

Finally for the fourth flag, we used a curl command in our powershell (curl is a command that basically asks the computer to fetch a file or some data) to access a website that we found called "http://scriptkidrev.ctf-league.osusec.org/credentials". We then used that command to scan for the data of "timmy" where we got the id of "1". We then deleted the information from file "1", and the status changed to that of the flag. 

## Reflection 
To be perfectly honest, I still don't fully understand all of this, as malware analysis, hacking C&Cs and deleting information from directories is all very advanced, but I definitely feel like I was exposed to the most information since the Deadface event. I really enjoyed this challenge and I'm looking forward to next week! I am glad that I got exposed to these ideas, as malware was one of the areas that I had no idea how to sovle in the Deadface event. I felt like I learned a lot and I am at the start of a long learning process. Super excited to develop in this area more!
