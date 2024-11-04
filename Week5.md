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
ELFs use a process called Linking & Loading in order to combine object files into an executable. It will then use the loader to read the headers and load the program into memory based off of what it reads. 


### Ghidra


## Overview



## Challenge of the week

## Reflection
