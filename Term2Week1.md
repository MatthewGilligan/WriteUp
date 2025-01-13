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

## Challenge of the week (1)
For the challenge of the week, called saloon, we ncat'd in and found a text file which contained a cool rhyme about how a stranger rolled up to a tavern and gave it a prompt about continuing with a (y/n). Hitting n ending connection, and y continued with another rhyme about how she needed to solve a math equation in order to pass. It asked us 60630 * 43458 = ? (1/100). I used a calculator, but I wasn't actually able to input anything and needed to start making a python script. In order to do this, we imported pwn and we also created a process p which took in the id for the challenge. 
### Our approach
Our approach to solving this challenge relied on sending the answer to a randomly generated math question 100 times in a row. We also needed to automate sending a y to the first question in order to continue with the challenge. We used the p.sendline and variations of this command in order to interact with the program directly. For a while we were wondering why sending just "y" wasn't working, but then we remembered that we needed to use bytes for all data. Our strategy was to read up until the end of the second rhyme, and save it as a variable called math. We then were able to manipulate the data by modifying our math variable. We also needed to remove the final 4 characters, as they were the parentheses that displayed progression. Additionally, we used the .decode command in python, which changes data from a byte to a string. With our data in string form we can modifiy it a little easier. We isolated the two numbers being multiplied by using the .split command. Because .split saves the values in an array, we were then able to multiple math[0] with math[1] in order to get our product. 
### First attempt
Our first attempt at beating the saloons code was: 
from pwn import * 
p = remote("saloon.ctf-league.osusec.org", 1308)

p.sendlineafter(b"(y/n)\n", b"y") //this line automates our sending of y after the yes or no prompt

p.recvuntil(b"math\n") //this reads until the end of the second poem

math = p.recvuntil(b"= ?") //saves math as just the numbers

math = math[:-4] //removes the last 4 characters
print(math)    //for readability, so that we know what's happening
math.decode() //conversion to a string

math.split("*") //splitting the math variable

math[0] * math[1] //multiplying the first and second elements of the math array

This attempt didn't fully work, and we went through a long debugging process. First of all, we didn't even have the for loop implemented. We debugged and found out that we were slightly overthinking, as well as not sending the correct result. We were using the first and second elements of the array, but we should've been using the 1st and 3rd, as the second wasn't actually where the number was located. 

### Second attempt
from pwn import *

p = remote("saloon.ctf-league.osusec.org", 1308)

p.sendlineafter(b"(y/n)\n", b"y") // send a line to progress

  

p.recvuntil(b"math\n") // reads until "math" (almost the end)

  

math = p.recvuntil(b"= ?") // takes the equation

  

for i in range(99): //the for loop, which repeats for the 100 times


    math = p.recvuntil(b"100\n")" //reads until the end of the question

  
    math = math.split() //splits it into multiple parts

    print(math)

    result = str(int(math[0]) * int(math[2])) // takes the product of the first and the third, which are the correct locations

    print(result) //readability so that we know that we're on the right track

  

    p.sendline(result.encode("utf-8")) //our encoding process that we ripped from ai. I had no idea what utf-8 was, but it ended up working. 

  

p.interactive() // so that we can interact with the program and read it. 
### 1st solution
After we implemented this code, we were met with the prompt "What's the secret message?" and we got a very long file with a lot of x's /'s and numbers. One of my teammates recognized this notation as being created with gimp. Gimp is an image editor, so we concluded that we needed to decode an image to get the first flag. No one on our team knew how to do this, but so we used copilot, which told us that 
"with open("image", "wb") as binary_file:

binary_file.write(file)" 
would work to decode the image.
After opening up the image, we found the first flag written on the image. 
### Second challenge
For our second challenge, we inputted the first flag as the "secret message" with sendline, and were met with another round of text. This one said "Can't come up with any more rhyming couplets
Uhh. Convert some numbers or you're banned from the robot bar
0xd6f56f4b as a base-10 integer is ? (1/100)"
Even I recognized this as a hexadecimal from earlier CTFs, which felt good because I felt like I was actually absorbing information. 

### Second solution
For the second challenge, we iterated upon our already made code. We then converted the actual hexadecimal (saying actual because we're not converting the bytes). After that, we needed to use a key to decode the second flag.

from pwn import * //This is all reused code
p = remote("saloon.ctf-league.osusec.org", 1308)

p.sendlineafter(b"(y/n)\n", b"y")

p.recvuntil(b"math\n")
for i in range(100):
    

    math = p.recvuntil(b"/100)")

    math = math.split()
    result = str(int(math[0]) * int(math[2]))
    p.sendline(result.encode()) 

p.recvuntil("here you go!\n")


file = p.recvuntil("What's the secret message?") 
 
#with open("image", "wb") as binary_file: //these are commented out as we no longer need to use them

       #binary_file.write(file)

p.sendline(b"osu{sh4rpsh00t3r}") //sending the secret message to start the second challenge

p.recvuntil(b"bar\n") //go until the end of the "poem"
for i in range(100): //for loop that covers all 100 
    

    math = p.recvuntil(b"/100)") //save the math variable to be the hexadecimal

    math = math.split() //splits it again into the actual integers

    result = str(int(math[0],16)) //saves the result variable as the first element of the math array converted from hexadecimal to a decimal, then turned into a stream.

    p.sendline(result.encode()) //we then send the encoded result with the sendline command 

p.recvuntil(b'key:\n') //because this resulted in the key, we needed to read until where it told us the key was.

key = p.recvuntil('message!\n') //we save the text of the key
message = p.recvuntil('goodbye!\n') //we then save the next part as a separate variable
print(bytes(k^m for k,m in zip(key, message))) //this results in the second flag, which we submitted for the points. Most of this decoding was done by my teammate or by ai

## Recap
This challenge was very fun, and was a nice welcome back for the winter term. I'm very grateful for the help that we got from the coaches this week, and I'm happy to have teammates to work with. I learned a lot about processes in python, and the basics of pwntools. I haven't really used python much, but I am growing to like it more and more. 
