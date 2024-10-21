# Week 3 challenge: super_Memory
## Matthew Gilligan

## Recap
This week was unfortunately the first week where I didn't find the flag within the time constraints and lost half credit on the second assignment. That being said, it was still a super fun challenge that was very creative. Week 3 and still going strong! This week started with us hearing that we were going to be reading in C, so I was excited, as C++ is my main language. Additionally, we were told that big numbers use 4 bytes of data. We had an example where we learned about what hexadecimal was, and how it used a base 16 counting system using "123456789abcdef". The first digit would be multiplied by 1, the second would be multiplied by 256, the third by 256^2 and the final by 256^3. This makes it so that each when combined will equal the original large number. All of these bytes are prefixed by 0x such as 0xa =10 or 0xc =12. 

After this, we went breifly over arrays and structs, and focused on how they can overflow. Arrays can hold more than 1 variable at a time and are defined by the square brackets. For example: int myArray[2] holds two values, however when we try to call value "2" from the array it overflows because it starts counting from 0, meaning that the array is made up of myArray[0] and myArray[1]. 

Arrays aren't the only thing that can overflow. Numbers can also overflow. A number that keeps adding to itself will eventually "wrap around" into the opposite direction (ex: 1,2,...126,127,-128,-127,-126...). 
## Overview

## Challenge of the week 

## Reflection
