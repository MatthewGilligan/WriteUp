# Term 2 Week 1
## Matthew Gilligan

## Introduction and challenge prerequisites
This was our first challenge back for the winter term of 2025, and it was really fun to get back into CTFs again! This week's challenge involved using the python library pwntools. I am not very familiar with python, but I was still able to understand the basics of the lesson and contribute to my team. 

#### Pwntools
Pwntools is a very useful python library that doesn't assume any specific data type, which makes it more flexible. Pwntools assumes all are bytes, which can be confusing at first, but is more direct and effecient. We installed Pwntools using Pip (which stands for python install package) pwntools. 

#### P = process
P = process is a way that we can use the "process" function and interact with that function using code. This was the basis of how we solved all of the challenges. We learned many useful tools that can be used to manipulate the process directly and output the data so that we can better understand what is happening. Here are a few of the commands we learned and what they do:
**p.recvall:** This stands for "receive all call" and will capture all of the output of a function. This function doesn't manually print the output, so you will need to use another line to print it. 
**p.interactive:** This is usually a more efficient replacement for p.recvall, as it also prints the output, and allows for interaction in actual real time. This is also a much more flexible line and allows the use of other types of data manipulation. (However, it is not STRICTLY better than p.recvall)
**p.recvline:** Similar to p.recvall, but it only captures the first line.
**p.recvuntil(b,"string"):** Another capturing call, but this time it reads until it reaches a certain string. We need to use the b, as we are casting it as a byte. 

#### Example of use
Given the process "bc" (which stands for basic calculator), we can try the input 3*4, which we know is equal to 12. However, when we use the code: ans = p.recvline() and print ans, we get "b'12\n'" as this is how it is stored. We can manipulate ans by casting it as an int (with int(ans)) or by multiple other ways that the example didn't go into.
Important example: If we have the line ""hello 1234 0x1234" and we've saved it as line, we can use line.split to give the result hello, 1234 and 0x1234. ".split" was a very important part of this challenge and I wish that I had used it earlier. 

#### Final bits of information
The last slide of the week was titled "knowledge needed for the challenge" and contained the following: 
OTP = one time pad = bytes of one thing xored with bytes of another thing. 
File signatures AKA magic bytes. 

With all of this information, we were able to begin the challenge. 

## Challenge of the week
For the challenge of the week, called saloon, we were given 
### Overview
### Our approach
### First attempt
### Second attempt
### How we solved it

## Recap
