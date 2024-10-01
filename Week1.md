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
This is more text (I will expand on this later)
