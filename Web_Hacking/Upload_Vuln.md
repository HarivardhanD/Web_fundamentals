- File upload vuln --> dangerous

- Can lead to Remote code access as well .

- So if we have a file upload point on a site :
    - First thing to do is enumeration [ IMPORTANT ]
    - Check different endpoints on the site using `GOBUSTER`
    - Also check how the user input is taken by website
    - Using burp to intercept upload requests
    - wappylyser can also give us a overview 


# FILE UPLOAD VULNERABILITY

### FILE OVERWRITE VULNERABILITY
- File upload vulnerability is a security issue where a website allows users to upload files, but does not properly check them.

- When we upload file , it gets stored on website .

- EXAMPLE :
    - SUPPOSE WE HAVE A DOGGY WEBSITE WITH `UPLOAD FEATURE` --> AND WEBPAGE HAS A DEFAULT IMAGE OF DOG [ FACE OF WEBPAGE ]
    - NOW I INSPECT THE WEBPAGE AND FIND OUT THAT IMAGE IS BEING TAKEN FROM ` images/dog.jpg1+` IMAGES DIRECOTRY
    - SO NOW, I UPLOAD A FILE OF `CAMEL` AND NAME IT `dog.jpg` AND BOOYAH THE WEBPAGE CHANEGS TO CAMEL .
    - THIS IS A SERIOUS CONCERN HERE , CAUSE :
        - The server is not checking the file name
        - It allows files with the same name to overwrite existing ones 

    - FIX FOR THIS :
        - Validate file names properly
        - Rename uploaded files (random name or timestamp)
        - Reject duplicate file names
        - Set proper file permissions (users should not overwrite important files)


    - WHY CAN IT BE DANGEROUS ?
        - It can deface a website (change how it looks)
        - It damages the website’s reputation
        - In some cases, attackers can upload malicious scripts
        - That can lead to full server compromise

### REMOTE CODE EXECUTION

- Here , we upload the file but instead of normal files, we upload malicious script.
- First we need to enumerate on what language is backend running and then based on that language we have to upload a malicious script in the same language
 - EXAMPLE:
    - RUN MALICIOUS CODE -> REVERSE SHELL CODE --> UPLOAD ON SITE
    - USING NETCAT CAPTURE THE SIGNAL AND THEN BOOYAH U GET TO ACCESS THE SERVER VIA UR TERMINAL



# FILTERING 

- Now lets learn how developers try to prevent this attack :
    - They use FILTERING TECHNIQUES :
        - THERE ARE 2 TYPES :
            - `CLIENT SIDE FILTERING`
            - ` SERVER SIDE FILTERING `

- `CLIENT SIDE FILTERING` --> RUNS IN UR BROWSER (JS) --> FILTERING HAPPENS AT UR END BEFORE IT IS UPLOADED INTO THE SERVER
    - EASY TO BYPASS --> AS SCRIPT IS ON USER-SIDE

- `SERVER-SIDE FILTERING` --> RUNS ON SERVER AND HAPPENS AFTER UPLOAD REQ IS SENT



- MENTAL MODEL :

    - POST /upload HTTP/1.1

    - Content-Type: multipart/form-data

    - ------boundary
    - Content-Disposition: form-data; name="file"; filename="cat.jpg"
    - Content-Type: image/jpeg

    - <actual file bytes here>
        - if jpeg : `FF D8 FF`
        - if png : `89 50 4E 47 0D 0A 1A 0A`
    - ----boundary--



- TYPES OF FILTERING BASED ON  THE ABOVE :
    1. EXTENSION FILTERING :
        - Here filtering is done on the extension (.jpg,.png)
        - whitelist --> only .jpg are allowed [ files allowed to be uploaded]
        - blacklist --> files not allowed to be uploaded 

    - WEAK : Because we can trick the extension .[ ex : shell.php.jpg ]

    2. MIME TYPE FILTERING :
        - MIME --> MULTI-PURPOSE INTERNET MAIL EXTENSION 
        - This is used to identify the file type transferred over HTTP(S)
        - This can be seen in ` content-Type:image/jpeg`
        
    - WEAK : Because u can use burp suite /POSTMAN to intercept and change the request beingn sent to the server

    3. MAGIC NUMBER FILTERING :
        - It checks the first few bytes of the file :
            - For `jpeg` it checks `FF D8 FF` and so on for other types

        - WEAK : Because we can in the start of script write the bytes followed by malicious script, hence bypassing it .

    4. FILE SIZE FILTERINIG:

        - it checks the sixe of the file and if file size is large, it blocks the upload.
        
        - WEAK : Attacker can write malicious code and minimmise the size

    5. FILE NAME FILTERING :
        - HERE WE RENAME THE IMPORTANT FILES AND MAKE SURE USERS CANT OVERWRITE IT.
        - REMOVE DANGEROUS CHARACTERS WHICH CAN LEAD TO NULL BYTE OR PATH TRAVERSALS
        - NEVER ALLOW USERS TO CONTROL PATH



- But in real world, we user defense in depth where multiple security methods are used .
- Hackers --> Dont fight the security , rather they live like normal users ,bypass the checks silently and once inside, They exploit everything they can .


# BYPASSING CLIENT - SIDE FILTERING

- There are 4 ways by which you can bypass the client - side filtering :
    - 1. TURN OFF JS IN YOUR BROWSER --> Wont work if the JS is req fir basic functionality .
    - 2. INTERCEPT AND MODIFY THE INCOMING PAGE --> Usiing burp-suite
    - 3. INTERCEPT AND MODIFT THE FILE UPLOAD 
    - 4. SEND FILE DIRECTLY TO UPLOAD POINT --> Using `CURL`--> `curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>` // some other time


- ### 1. DISABLING JS FOR CLIENT-SIDE FILTERING 
    - First I went to webpage and intercepted and found JS was ran on client side [ IN INTERCEPT I SAW JS CODE , which gave me hint that filtering might be client side ]

    - Then on the : `http://java.uploadvulns.thm` i captured the request and then in the proxy with intercept on , i right click --> `DO INTERCEPT` --> `Response to this request` and then clicked `forward` . 

    - Then i got the response on burp and i removed the `client-side JS` and clicked `forward`.

    - Then i turned off the foxyproxy and hence now i could upload any file since the `JS - Client side filter` was disabled , but when we reload this resets back.


- ### 2. INTERCEPTING THE REQUEST AND MODIFYING IT ON BURP.
    - first created a dem.png file for upload .
    - Using nano , inside dem.png i pasted a rev-shell code and turned on NETCAT
    - Then on browser , selected file and turned on proxyfoxy
    - clicked upload and captured the file before submitting it to browser for filtering
    - Then , changed the file type from `dem.png` to `dem.php` and content type from `image/png` to `text/x-php`.
    - Then forward the request to the brower and hence uploading the `dem.php` file





##

# BYPASSING SERVER-SIDE FILTERING

- So i first created a `dem.png` , `dem.php` and `dem.jpg` of which `.png` and `.jpg` could be uploaded

- set a netcat listener --> `nc -nlvp 1234`

- Then , i tried uploading `dem.php` --> couldn't upload
- So `.php` was being filtered
- Then i tried ` dem.png.php` --> no upload
- Then i tried differnt extensions for php like phtm etc and for this finally `dem.php5 ` worked and also `dem.php.png` could be uploaded
- This gives conclusion that --> only the last `.extension` is considered for filtering and also it only filters few `.php` extensions


##

# BYPASSIING SERVER-SIDE FILTERING --> MAGIC NUMBERS