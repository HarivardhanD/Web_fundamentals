# JUICE SHOP

- Juice shop is for practical execution of OWASP - TOP 10 web-vulnerabilites
- We will be doing the following rooms :
    - Injection
    
    - Broken Authentication

    - Sensitive Data Exposure

    - Broken Access Control

    - Cross-Site Scripting XSS


- First opened the OWASP juice shop
    - Just went through everything 
    - collected email and endpoints
    - input places
    - URL Parameter

- Types of Injection :
    - `SQL INJECTION` --> injecting into the database
    - `command injection` --> injecting into server/systems
    - `email injection` --> email cant filter the malicious attacks .

- SQL INJECTION EXAMPLES:
    - In the burp capture request -> send to repeater --> in the email id / pass add the following SQL command for testing sql injection :
        - {"email":" a 'OR 1=1 --"}
            - Here ` 'OR 1=1 --` is sql command where :
                - `1=1` means always true . ie sql first check our entered email ie `a` then `or` will check `1=1` which is true hence making our email correct
                - `--` is comments which comments nay command after `--` 


- In burp suite intruder is used to brute force the password / email on the web-site pages and positions u wanna brute force use ` § `


- Sometimes when we try to download files we might face an error : ` ONLY .md AND .pdf FILES ARE ALLOWED !` 
    - So lets say we have a file : `package.json.bak` which is neither .md nor .pdf
    - but we wanna download it
    - In such case we can use ` POISON NULL BYTE ` which is ` %00 `
    - The `Poison Null Byte` will now look like this:` %2500`. Adding this and then a `.md` to the end will bypass the  error!
    - So what is does is it will read the file till null terminator and then past it ,so the url will look like this :
        - `https://ftp/package.json.bak%2500.md`
        - `%25` is used with `00` to encode it into url format !



- `Horizontal privilege escalation ` --> Gaining access btw different users of same privilege

- ` Vertical privilege escalation ` --> gaining access of higher priviliged user

- Now i wanted to know differnt api endoints on OWASP juice shop :
    - Did ` inspect ` on the page 
    - went to debugger
    - then searched for `path` where i got differnt api endpoints of which we had administrator
    - then i in the url tried moving around , but couldnt access the endpoint
    - Then logged in as admin and moved to the page --> successfull !


- `XSS` --> `CROSS SITE SCRIPTING` --> Allows us to run JS in web applications

- TYPES OF XSS ATTACKS :
    - `DOM` -> DOCUMENT OBJECT MODEL --> Here we use JS Inside HTML to execute on the webpage [ web pages blocks direct js commands ] 
        - EXAMPLE : `<iframe src="javascript:alert(`xss`)">`
            - This used `iframe` which is HTML and inside this we used `JS`

    - `Persisten XSS`--> when u load the page, JS gets executed [ BLOG POST ]
    - `Reflected XSS` --> When u run JS in the webpage , the OP is reflected there itself


