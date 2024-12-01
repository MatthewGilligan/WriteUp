
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
There was a website. We entered various prompts in order to find the 3 flags

## Reflection 
This was a really fun week. Even the AI wouldn't help me learn SQL injection, so it was really cool that I got to learn the basics. I will make sure to be ethical about this new skill and not hack into stuff (not that I could even if I wanted to)
