# BURP BASICS

- BURP-SUITE IS JAVA-BAES FRAMEWORK FOR CONDUCTING WEB-APPLICATION PENTESTING

- ### Burp intruder can be used for login page password brute-forcing
- It is used for hands-on security assessments of web and mobile applications, including those that rely on application programming interfaces (APIs).

- Burp Suite captures and enables manipulation of all the HTTP/HTTPS traffic between a browser and a web server OR even before it reaches the browser, we can intercept,view and modify web-request.

- We will be using BURP-SUITE community addition , which is free :
    - `PROXY` -->  It enables interception and modification of requests and responses while interacting with web applications.
    - `REPEATER` --> Repeater allows for capturing, modifying, and resending the same request multiple times 
    - `INTRUDER` -->  utilized for brute-force attacks or fuzzing endpoints.
    - `DECODER` --> decode captured information or encode payloads before sending them to the target
    - `COMPARER` -->  comparison of two pieces of data at either the word or byte level.
    - `SEQUENCER` --> Burp Sequencer is a tool that checks how random session tokens (cookies) or security tokens are


- The `Burp Suite Extender` module allows for quick and easy loading of extensions into the framework, while the marketplace, known as the `BApp Store`, enables downloading of third-party modules


- SHORTCUTS :
    - `Ctrl + Shift + D`	Dashboard
    - `Ctrl + Shift + T`	Target tab
    - `Ctrl + Shift + P`	Proxy tab
    - `Ctrl + Shift + I`	Intruder tab
    - `Ctrl + Shift + R`	Repeater tab


## BURP INTRUDER

- used for fuzzing attack !

- Four attack types :
    - `SNIPER ATTACK` :  Sniper attacks iterate through all the payloads in a linear fashion, allowing for precise and focused testing.
    - `BATTERING RAM` :  Sends all payloads simultaneously. useful when testing for race conditions or when payloads need to be sent concurrently.
    - `PITCHFORK`     :  Pitchfork attacks are effective when there are distinct parameters that need separate testing. 
    - `CLUSTER BOMB`  :  Combines the Sniper and Pitchfork approaches. useful when multiple positions have different payloads, and we want to test them all together.

## 

- First i logged in website, where it needed -- `username` and `pass` for logging in
- so i used the `PITCHFORK` of the intruder, so initially i used proxyfoxy to capture thr request and then added the special symbol as follows:
    - username=`§123§` & password=`§123§` --> smth like `s` is the special symbol and after adding it , i went to the positions, selected bth payload 1 and 2, one by one and added wordlist for username and password respectively for both of these. [ THE PAYLOAD TYPE USED HERE WAS --> `SIMPLELIST` THIS LETS ME ADD WORDLIST FOR FUZZING ]

## 

- Now, i had to check the endpoint , ie `/support/ticket/NUMBER `, There is a flag, btw 1 to 100 endpoints, now for this i did the below:
    - went to the website, and captured the req via burp
    - added the special character , where the fuzzing is required, in this case its : /support/ticket/`NUMBER`
    - Then i went to the payload type and chose `numbers`
    - where i chose : FROM = 0
    - TO : 100

##

- ANOTHER PRACTICAL CHALLENEG : Here i had to fuzz usr and pass , same as first one but here there were : `CSRF TOKENS` and `SESSION COOKIE` were used to prevent fuzzing / brute force, so everytime we brute force usn and pass, we need matching fresh token and cookie. For this, specifically for getting fresh `cookie` and `token`, evrytiime i fuzz, i need to send GET to the browser, which give me fresh cookie and token and then i neede to brute force.So inorder to solve this, i used `macro`
    
    - 1.  burp --> setting --> `session` -> scroll to MACROS and press ADD --> a menu will appear, we need to select `GET`req for endpoint we are fuzzing and if doesnt exist, go to that page and capture the request --> then click OK.
    #
    - 2. Now , in the `session` settings , go to `Session handling rule` --> ADD --> SCOPE --> check use suite scope ( make sure in the scope , we have the main website URL) --> then go back to the `details` --> choose from dropdown `MACRO` and then choos update only foll param and URL session and add ` Session` and `LoginToken` --> OK
    #
    - 3. Then , come back to intruder and do the brute-force for username and password
            -  If you see 403 errors, then your macro is not working properly


    

##

## Other capapbilities of the Burp Suite

- DECODER : 
    - we can encode , decode, hash and smart decode the values

 
 - COMPARER : 
    - when performing a login bruteforce or credential stuffing attack with Intruder, you may wish to compare two responses with different lengths to see where the differences lie and whether the differences indicate a successful login


- SEQUENCER : 
    - Sequencer allows us to evaluate the entropy, or randomness, of "tokens"
    - There are 2 methods for this :
        - Live Capture : 
            - Send a request that generates a token (e.g., login → session cookie).
            - Burp Suite Sequencer automatically repeats the request thousands of times.
            - It collects many tokens from responses.
            - Then analyzes randomness / predictability.
        - Manual loading :
            - we manually enter the tokens and then analyse it ( stored token ) 