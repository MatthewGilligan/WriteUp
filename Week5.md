# Week 5
## Matthew Gilligan

## Recap

This week was really fun, as we got to do reverse engineering. This was exciting for me, as reverse engineering was another area of the Deadface event where I got stuck and didn't have any idea how to solve it. Before class we needed to install Ghidra, which is a reverse engineering tool made by the NSA. We went over executables, and how they can use ELF (Executable and Linkable Format) formatting, which contains machine code in the .text part of the ELF. 

### ELFs
ELF is a common file format for executables (any file that contains a program), object code (the "intermeditate level" output compiled from high level code into machine code or bytecode. The computer can process it, but it is not yet an executable), shared libraries (collections of code/data that can be used by multiple programs simultaneously), and core dumps (captures of a program's memory at a specific point, usually at the time of a crash). ELFs contain multiple sections such as:

##### Headers:
Describes the segments of the program that are loaded into memory. Each entry specifies where the segment is located in the file, and at what exact location it should be placed into the memory. 

#### Data sections of ELFs
###### .text:
Contains the executable code.

###### .data:
Contains initialized data.

###### .bss: 
Contains uninitialized data.

###### .rodata: 
Read-only data, such as constants.

###### .comment: 
Metadata or additional comments. 

##### Section Header Table: 
Describes the sections and their attributes (size, location, alignment)

#### How ELFs actually work
ELFs use a process called Linking & Loading in order to combine object files into an executable. It will then use the loader to read the headers and load the program into memory based off of what it reads. You can think of an ELF file like a recipie book: outlining what resources a program needs, telling the computer how to load the program into memory and run it, and all of the instructions to make it work.

### Ghidra
Ghidra is a reverse engineering tool devleoped by the NSA, and allows for people to analyze code even if they don't have the original source code. Ghidra takes an ELF file "rips it apart limb by limb" and turns it back into C code. This causes the memory and string compare to be really messed up, but allows for a "guess" of what the original code looked like. 

#### My checklist that I made during the Ghidra demonstration
You get to a window 

new project
no share
name it
create project. (stored in a folder)
hit "i" (for insert) 
don't read the stuff (it's a lot of stuff)
load it
takes you to a new screen
Elf on the side
functions in the middle
right is what it thinks things are after being compiled

//get a demo file
You can run a demo file
it asks for a password

Ghidra reads a input and uses check password function 
Ghidra reveals function
function says "if input == superSecretPassword" 
we can determine that superSecretPassword is the password
BANG! We're done

#### Jared's actually good notes that he took during the Ghidra explanation
Ghidra does this:

1. Read ELF (Executable and Linkable Format binary files)
2. Disassemble to Assembly
3. Decompile to pseudo-C code (tries its best but it is heuristic)
4. Analysis
    
    1. Data flow
    2. Symbols, functions, etc
    
5. Interactive annotation

### Summary of Ghidra
Ghidra takes in ELF files analyzes the file and breaks apart the components of the file. It then dissasembles the binary code, and converts it into assembly language for readability. It then decompiles the assembly into C, making for even better readability. Ghidra then has a user interface where you can navigate through the code and allow you to understand how it works. 
Ghidra is super important for finding vulnerabilities, understanding malware behavior and making defenses against potential threats. 

## Overview
Before we could start the challenge this week, we needed to install Ghidra, the software that was mentioned in the recap. In order to do this, we also needed to install a JDK, or Java Development kit. Also, in order to access the challenge, we needed to use the linux command "nc [server name here]". "nc" stands for "netcat" which allows you to connect to a server, however it doesn't work on the windows powershell, only linux. To get around this restriction, I went to a website called "nmap.org" and installed an extension, which allowed me to use "ncat" in place of "nc". This allowed me to access the 2 weekly challenges.


## Challenge of the week

### Challenge 1
For this week we needed to solve two challenges given a link to a file and the text: "rock paper scissors is a lot easier if ur opponent lets you read their mind before u start playing". I downloaded the file, went through the checklist, naming my project "first_chal". After opening the code and going through main, we found a function called "throwHands" with the inputs of "rock", "scissors", "paper", "paper", "scissors" and then printed "wow good job Here's your flag: %s\n". This function would only continue if the input would "beat" their call. Rock (tracked by 0), was beat by paper (tracked by 1), which was beat by scissors (tracked by 2), which of course was beat by rock. 
After obtaining the inputs that the machine would use, we went into the powershell that we accessed through ncat. We were greeted by a function that asked for a name, took in your input and said "hello [userInput]". It then prompted us to play rock paper scissors using 0,1 and 2. I entered 1, 0, 2, 2, 0 sequentially in order to beat the machine, and it then printed out the flag. 

### Challenge 2
Challenge 2 seemed very similar to challenge 1 at first, as when I accessed the file I was once again asked to enter a name. However, when I entered my name I was met with the response: "Please enter a longer name". Checking in Ghidra, we found a statement after it took the input of name that said: "if name < 0xb: [continue the program]". 0xb is the hexadecimal for 11, meaning that the name needed to be 11 digits long. It then took this name as a character array into a function called "make_moves" which outputted the rock paper scissors decisions. 

#### make_moves function
This is the exact C code for make_moves:

void make_moves(char *in,int *out)

{
  int *out_local;
  char *in_local;
  int delta;
  int i;
  
  delta = 0;
  for (i = 0; i < 10; i = i + 1) {
    in[i] = in[i] + (char)delta;
    out[i] = (int)(in[i] % '\x04');
    delta = (in[i] + 0x1ca3) % 0x15;
  }
  return;
}

#### Understanding the function
The first thing that we recognized was that if it was handed the exact same characters for the "in" variable then it would always create the same output. We know that character arrays convert the characters into ascii values, with the value of a being 97. Additionally, hexadecimals are used for the line that calculates the delta variable. 0x1ca3 correlates to 7331 and 0x15 correlates to 21. This adds 7331 to the ith value of the in array, then divides by 21 and assigns delta with the remainder (modulo 21). Also, x04 representes 4, which divides by 4 and takes the remainder. 

A key part of this function is that it reassigns the value of the in array by adding delta to ith value of in. This means that even though our string is all the same letter, it will not output the same thing for every iteration of the loop. 

To solve this challenge, our group of four split off into two seperate groups, with one doing the math manually and another just trying to brute force it. While the brute force team did end up finishing first, my group with the math was able to fully understand the code and how our data was transformed. The out variable is assigned second, and always results in a number between 0 and 3, which will correlate to the rock paper scissors outputs. With our original string of "aaaaaaaaaaa", the code would result in a 3 being output for the 7th value, as out was being assigned to 99 % 4, but by switching the 7th value to "b" (with an ascii value of 98), we can make sure that all outputs are between 0 and 2, allowing us to beat the algorithm. (The group with all "a" also managed to beat the algorithm, but with just testing and checking). 

## Reflection
Overall, this week was super fun and super informative. I was super excited to learn about Ghidra and ELFs, as well as get first hand experience reverse engineering code. I am very excited to be exposed to these concepts and ideas, and I definitely found this challenge super fun. I certainly want to develop in this area more, and I'm excited to keep learning!
