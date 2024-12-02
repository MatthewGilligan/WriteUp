# Week 8
## Matthew Gilligan

## Recap
This week we went over crytography, more specifically, into ransomware. Ransomware is a type of malware that infects a users files and locks them behind an encryption that only the hacker has the key to. An important quote from this week was "Cryptography turns multiple complex problems into key management problems". 

### Logic
The first thing that we covered in this week's slides was logic, and how it works for encryption. Common examples of logic are "and" (both are true), "Or" (either are true, with the possibility of both), "Not" (exactly what it sounds like. Apply before another logic check), and "XOR" (either are true, bot NOT both). 
Logic is especially useful for binary, because like how logic is either true or false, binary is either a 1 or a 0. 1 evaluates to true while 0 evaluates to false. Using thi, you can make "logic gates" in binary, where two binary strings are compared against each other using the logic of the gate and one new binary string is outputted. For example, an XOR gate evaluates to true if either string a 1 at the index, but not both. So the strings 1100 and 0110 would become 1010 because you check each bit against the other bit at the same index. These XOR gates are very useful and commonplace in cryptography, as they are self-cancelling and are able to be reversed if you have the original two strings (You can also use more than two strings and XOR gates will still work)

### Encryption
Using one of these XOR gates, you could hypothetically XOR a file with some random junk that only you know in order to encrypt it. This is called a "One time pad" (OTP), which is theoretically unbreakable because the ciphertext doesn't actually give away any information about the encrypted text (assuming that the key is actually random and only used once). 
A major pro for using these OTPs is that they are usually very secure, and no one can read your encrypted file without your key. However, a potential con with this approach is that it is vulnerable to collisions. Collisions are where two different inputs produce the same output. This means that with enough "brute force" attacks using the right logic, someone could replicate the so-called "unreplicable" key. 

### File Magick
Another thing that we covered in the introductory slides was file magick. File magick is the meta data assigned to each type of file extension. This data is displayed in the form of a sequence of numbers called "magick bytes". Some files will have multiple types of bytes while others will only  have 1 sequence. ELF binaries will always start with  7f 45 4c 46, while ZIP archives always start with 50 4b. One thing to consider and be aware of is that some of these files may have magic bytes near the beginning of the file, but not directly at the beginning. The best way to determine the magick bytes of different types of files is by using wikipedia. 

### Hash functions
Hash functions are mathematical algorithms that take an input 


## Overview
3 flags again. We didn't find the last one in time.

## Challenge of the week
Really hard stuff this week. 

## Reflection
Wow, crazy cool week. Learned a lot. My first intoduction to cryptography. 
