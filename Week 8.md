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
Hash functions are mathematical algorithms that take an input, and return a string of bytes as an output. They transform data and create a unique output for a given input, making them very useful at securing information. The main properties of hash functions are that they are deterministic (produce the same output for the same input), have pre-image resistance (it's very difficult to find the original input), and are collision resistant (it should be very hard to get the same output with the same input). All of these factor mean that a small change in the input can create a drastic change in the output. The hashes that we went over were MD5, sha 1, sha 3 and poly1305. Of these, md5 and sha1 were considered broken, and poly1305 is considered kind of broken. A hash being "broken" means that it's trivial to compute collisions for the hash, making them less secure. 

## Overview
For this week's challenge, the recurring character Timmy and his friend Jimmy both got hit with ransomware. We needed to recover these files and find the 3 flags hidden in the various files. We needed to decrypt a pdf and a jpg. This challenge required real ransomware that was commented out. If at any time you deleted the comment from the top of the ransomware file and ran it on your computer, you would get hit with actual ransomware without a key to restore your files. It was made very clear to not run this ransomware, to not distribute this ransomware and to just read the script and analyze what it does. 

## Challenge of the week
For the challenge of the week, all we were given was a zip file that contained a text file as well as two other encrypted files and a python script that contained the malware. The text fille told us that "you've been mogged lol" and gave a link to a website with id of 3 to recover the files. Going to the website, it told us that told us to pay "5.3 sigmacoin to the address of _N2XNQAFHZ7ZVY7OFDXAVYNMN27KLGBSEFD6GK5XHLTATZFJI36UA_ by 2024-11-24" to get the files back. Part of our team tried to track down the address while the other team searched through the file. Eventually, both halves of the team merged in order to look through the file as we realized that the address wasn't relevant. 

Starting out, we identified an essential part of the python script that was the variable "secretKey". Secret key was an mp5 hash that took in the hostname, the str value of "randomThing" (a randint between 1 and 1333337) and the time. This hash was then used in a function called "Encrypt()" that XOR'd the contents of the file with the hash and also added ".ohio" to the end of the encryted file. 

### First attempt at breaking the hash
For our first attempt at breaking the hash, we set up a for loop that iterated from all integer values from 1 to 1333337 (the possible values of randomThing), checking the md5 hash with the logic of md5(timmy + str [i] + "2024-11-24".) This hash didn't work for multiple reasons, the main one being that the hostname was not timmy. Also, as we would later realize, the time was also off. 

### Searching for the hostname
Looking through the ransomware file, we found a line in part of the encrypt_and_upload function that uploaded the file as "victim" + the id. Going back into the website, we appended "/victim3" (as we knew that the id was 3 from the start of the challenge). This took us to a new page, which displayed the hostname, username and time. It turns out that the hostname was actually the first flag, which we submitted. 

### Second attempt at breaking the hash
After finally finding the hostname and correct time, we were able to reconstruct our for loop. This loops went through all of the values from 1 to 1333337 and compared our hash with str value i to the original hash with an unknown str value. Eventually, at i = 502651, our loop returned a match and stopped. Now we finally had the complete md5 hash. 

### De-encrypting the pdf
After we had the complete hash, we put it back into an XOR gate that we created in python. This code opened the encrypted pdf file, and saved it in a temporary array called contents. We then decoded the contents array with the XOR gate and hash and made a new, un-encryted pdf. In this file was Timmy's "hoemwork" with the flag written on one of the questions. 

### De-encrypting the jpg
To start decrypting the jpg, we repeated the process of that we used for the pdf, but it didn't work because of our magick bytes that we were using. The magick bytes for a pdf and a jpg are differnt, and a jpg can use two different sets of magick bytes. Additionally, the bytes are offset by 4. We originally were decrypting the index i-4 to compensate for this, but we found a less convoluted way by having the reader copy from the 4th index onward. This allowed us to decode the jpg to find an image of skibidi toilet and a text file that had some chatlogs between jimmy and timmy. 

### Finding the final flag
This final flag was very difficult to find, and my group actually timed out in solving it, only getting half credit. One thing that we realized was that we were looking for jimmy's information,not timmy's. We were using the wrong victim id. We manually altered the victim id in the website until we found Timmy at id 17. This gave us the new hostname "jimmypc" and a new time. Additionally it prompted a download. Originally this download didn't prompt, and we got stuck, but after some help, we found a file called "super_important_video_final1.mp4.ohio". These magick bytes were very hard to decode, and we ended up using the correct bytes for a while. We did multiple iterations of our brute force code using different combinations of the magick bytes until it worked. We iterated until we found the index of the str value of the hash (1234994) then used that new hash to decrypt the mp4.  

### Rick-rolled
Of course: The final flag was buried in a rick roll video with it at the bottom of the video. We watched the full rick roll and then we submitted the final flag for half credit. 

## Reflection
What a super fun week! I feel like I learned a lot, and I really persisted through a challenge that was very difficult for me. While helping us with our magick bytes issue Troy gave us 10 "gold stars". I will treasure these imaginary stars forever.
Overall a super interesting week that I really enjoyed.
