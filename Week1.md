# Arcade Challenge Week 1 
## Matthew Gilligan

## Recap
This was my first ever CTF! I'm very happy to say that our group was able to find both flags, and I certainly learned a lot about the basics of detecting vulnerabilities. While at times it felt like drinking from a firehose, I really enjoyed the first day at CTF and I'm super grateful for all the help that my group gave me!

## Overview
Our first assignment was to hack into an arcade website called the Geriatric Gamber's Emporium, which featured two games and two flags hidden within the games. We were given access to the frontend and backend, which meant that we were able to not only see the code of the webiste, but also able to see the code from our server. Normally we aren't able to see the serverside code, but today, seeing as it was our first meeting, we could use this resource. The frontend and backend are the main two "points of attack" when hacking and the main two areas of defense when working in cybersecurity.

## Part 1: Breakdown game
For the first game, we were presented with a game in which there was a ball, a paddle and 30 breakable blocks. When the ball hit any surface it would "bounce" off and grant a point. However, the catch was that the ball moved painstakingly slow, and it was much more effecient to find the vulnerability rather than complete the game. In order to access the code, we used Inspect Element, accessing frontend code. After opening up inspect element, we were presented with a menu of different tabs, each with different functions. While I was only minimally familiar with inspect element, I was able to find the sources tab, under which was "sources.js", or showing the javascript code for the game. My initial thought was to change the values of this code, but I soon realized that my edits to the javascript file weren't showing up in the main website, and that I needed to use a console command to access the flag. 
To find the flag I searched for "flag" and found that the flag was located in an if statement that would return the flag if presented with a score over 30. I have never coded with javascript before, so the syntax was confusing, but I understood the general logic of the if statement. After consulting as a group, we determined that we needed to create a value with a score value over 30, then call that value, tricking the game into thinking that we had beaten it. In order to acheive this, we used the code: 

const data = await fetch('/breakout?score=31'); 
			alert(await data.text());

This is new syntax for me, but the way that I understand it is that fetch makes "network requests" which directly takes or asks for information from the website. Await is just something that Javascript uses in order for it to execute, and alert is the way that we get the website to print the key to us. Asks our new const to take the value of 31, takes the key value and then prints the text part of the const. 
This two-liner was able to get the breakout game to print out the key, which we submitted for 40 points. 

## Part 2: Raffle game
For the second game we were presented with a roulette style game, with a 5 slots, each with the potential to generate an integer from 0-9. There was an area for player input that would only accept integers, with two arrows that could raise or lower the integer. Seeing as this game would take hours to get the flag by just guessing, we needed to hack in to get the flag. This time, we found that the code was located in the backend serverside code. We downloaded it from the link given to us in the Discord message and began examining how the game functioned. We found out that at the start of the game 5 numbers were randomly generated, then saved in an array and compared to a player input to check to see if the player guessed all 5 correctly. We searched for the flag and found that it was gaurded by 5 separate if statements. These if statements checked 3 things, and if any of them were TRUE would deny the flag. This meant that if all of them were false, we would get the flag. The if statements checked:

1) If a player input didn't exist
2) If the player input was greater than the value of the number stored in the array slot
3) If the player input was less than the value of the number stored in the array slot.
   
Both condition 2 and condition 3 used the "parseInt()" function in JavaScript in order to get the number for the player input. 
We thought that it was odd that it didn't just check if the player input was equal to the randomly generated number, so we examined a little closer, and one of us realized that the parseInt() function will always evaluate to false if fed a non-integer value.
We used a similar strategy as the first flag, creating a const, assigning it with values, then calling that value, making the game use our generated value as opposed to randomly generated values. Our code was:

const data = await fetch('/raffle?field1=@&field2=@&field3=@&field4=@&field5=@');
alert(await data.text());

We used the "@" symbol, but any non-integer would've worked. So long as it made the parseInt() function evaluate to false. We needed to fetch (ask for website information) with our 5 values as substitutes for the 5 parameters taken by the if statements. After alerting and causing the webiste to print, we obtained the key, which we submitted for 60 points. 

## Reflection
I'm very happy to have been able to solve these first exercises with my group, and I'm incredibly grateful for all the support from my team and the coaches. I'm looking forward to next week's flags and I hope that I get another great group to work with and learn from!
