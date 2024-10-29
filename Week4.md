# Week 3 challenge: super_Memory
## Matthew Gilligan


## Recap
This week we learned a lot about malware and even how to hack the hacker back. This was super cool and new to me, as I had never done this before, and I also got stuck on this during the Deadface CTF event. We learned specifically about C# Malware analysis, where you find the code and examine what is does to the computer. 
C# malware analysis involves 
1) collection: Gathering the malware/ related data (logs or network traffic)
2) Static analysis: Examining the malware without executing it. This involves the use of tools (we used dnSpy, but I'll get to that later)
3) Dynamic analysis: Running the malware in a controlled environment and observing the behavior. (I installed a virtual machine called VMware)
4) Code analysis: DECOMPILING (This was the main thing we did this week). Decompiling allows you to see the source code and analyze the logic behind it. It goes from the code being told to the machine back into "normal" code.
5) Behavior analysis: Monitor the malware in real time and observe how it interacts with the network and system. (Helpful tools I've yet to install: Wireshark)
6) Reverse engineering: Basically remake the malware, and break it into little parts to better understand how everything interacts.

Decompiling was the main thing that we were doing this week. In order to decompile, you need tools that you can install onto your computer. For this week, we used a tool called dnSpy, which takes code and transforms it back into normal code. The reason that we are using C#, is because it uses bytecocde as opposed to machine code. Machine code is the thing that goes directly into the computer, like assembly, while bytecode needs to go through another interpreter before being transformed into machine code. It is a "higher level" language that has more safety nets and features, but might not be as fast. 

After we had our dnSpy installed, we needed to drag a .dll file into it. A dll file stands for dynamic link library, which is like reusable code. Similar to a real life library, dll's "borrow" stuff without duplicating them. They borrow functions and resources and can be shared across multiple programs like a storing space. 

## Overview

## Challenge of the week

## Reflection 


