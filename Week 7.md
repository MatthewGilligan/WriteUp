
# Week 7
## Matthew Gilligan

## Recap
This week we went over website vulnerabilities, and the most common way that they are exploited: SQL Injection. SQL injection is a type of cyber attack where the hacker inserts SQL code through an input box, most commonly a password reader. 
This meeting, we first went over what is inside a website, and how it has a frontend (what the user interacts with), and a backend (what actually runs the website). We talked about how the backend has a webserver (usually with a javascript node) and a database. This database is incredbily important for the functionality of the webiste, as it allows for the loading of different contents, and also stores the users and their passwords. The webserver will run a query through the database, and the database will say "yes" or "no" and return different values depending on the contents of the query.

### SQL
We then moved on to SQL, or Structured Query Language, also called sequel. This allows for the selecting of different columns from a table with some logic statements (typical logic statements are WHERE, AND, OR). The most common use of SQL is in passwords, as a log in request basically says: Select all from the table "users" where the username is equal to 'username' and the password is equal to 'password'.
You can add comments onto your SQL code with the use of the double - mark, "--". Comments at the end of a line are how you get your code to actually bypass the security measures, with an "Entry point bypass". If you leave your statement open, and it bypasses the entry point detection measures, you can put your code directly into the database.

### Information Schema
Information schema is a second, auxillary database inside SQL. It uses meta data, and is absolutely massive. It is a read-only database, and is basically like a make or blueprint of the database. It helps explain how the data itself is organized without actually showing the data itself. It is very useful for injection and attacks, as it can be used to reveal table names (essential for SQL queries), identify sensitive data and even identify paths for privilege escalation. 

### Union select 
SQL allows you to select multiple things at a time using union select. For example: If I select stuff from one database, I can union select from another table and get all the queries that match that data in the other table. (In real SQL; Select * from users union select table_name from information_schema.tables; )
Sometimes you will get an error because your select statements have a different number of columns. In order to avoid this error, use NULL in the parameter for the column that doesn't matter or doesn't exist.

## Overview
This week we had three flags that we needed to find through breaking into an example website that had a button that recorded each button press, a place to enter a username and password, and a recipie that told you how to properly press buttons. In order to find these flags, we needed to log in to the actual website that was hidden behind the username and password check. 
For additional resources and help, we were given the webisite https://book.hacktricks.xyz/pentesting-web/sql-injection , which listed out additional tips and strategies for using sql injection to bypass entry point detection. 

## Challenge of the week
After going to the webiste, our team quickly reaized that we should try to log in as an admin in order to gain better information about the website and unlock more permissions.

### Flag 1
I used the SQL injection 'or 1=1; -- in order to make the password evaluate as true, confusing the website to let me past the initial detection. This revealed a database of all of the people who were logging in. In order to fully see the database, we used the command "'select * from users where username = '' and password = 'pass' or 1=1 -- word'" which revealed everyone logging in. We were told that we were supposed to look for anything out of the ordinary, so when one person named themselves "flag", it really threw off our group. We spent a lot of time tying to get the log in details of that user, until we realized that we were supposed to get the log in details of the admin account. After realizing that we needed to log in as the admin account, we stopped going after the fake "flag" and went back to looking for the admin. 
We used the command " ' or username='admin'; -- " in order to get the admin log in. This worked because the command becomes select * from users where username='random username' password='' or username='Admin'; -- ' which will select the admin user. The admin user was the first flag, which we submitted and got the points for. 

### Flag 3 (the second flag we found)
For the second flag that we found, we accidently found flag 3 first instead of flag 2. This was apparently pretty common, as flag 2 was pretty easy to miss, and flag 3 was further down that path that we were already going on. After we had the admin log in, we were able to use "Admin'; -- " as the log in for the admin account, which gave us more permissions and information. We accessed the informtion schema tables with the add on of "from information_schema.tables". We used the command "' union select table_name, null from information_schema.tables -- " We then found a list, which listed out the table names, one of which was called "secret". We found that the secrets table looked like "username: secrets, score: null". We accessed the information of secret by using the command "' union select secret, null from secrets -- " which revealed the secret, which was flag 3. 

### Flag 2 (the third flag that we found)
For the third flag we found, we went back into the normal users table, and with a hint that was put on the board to "find the usernames and passwords" we used the command ' union select username, password from users -- we were able to find the usernames and passwords from all of the users. One of these users had the flag as their password, which we submitted for the full points. 


## Reflection 
This was a really fun week! I remember back in the deadface CTF event I was very stumped about how to break into websites, so it was really fun and interesting to learn how to use basic SQL injection. Even the AI wouldn't help me learn SQL injection, so it was really cool that I got to learn the basics. I am definitely going to be using the hacktricks website, and I'm excited to learn more!
