# AUTHENTICATION BYPASS

- Learn how to defeat logins and other authentication mechanisms to allow you access to unpermitted areas.

## BRIEF


- Here we will learn different ways via which authentication can be bypasses , broken or defeated


## USERNAME ENUMERATION

- So , in this section we had to find the usernames already existing in the webpage, so how to do this ?
- So lets take a website and put "admin" as username and fill other forms randomly, now as we know if user exist then the website will say , user already exist , yes this is our loop !

- So what we will do is using `fuff` we will use multiple usernames and filter the username which give us error "username already exist ".

- Hence, we will get list of username to try passwords on !

- Command : `fuff -w wordlist_path -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u Target_url/signup -mr "user already exist"`

    - Here `-X` will give us the METHOD used

    - `-d` data we are going to send
    - `-H` will let us add headers
        - Here we used content-type for -H because we are filling form 
    - `-u` set target URL
    - `-mr`[match regex] this says, tell me when "user already exist message" pops up
    - In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request.

- Why u use POST? 
-> Whenever u are sending message to server , u need to use POST METHOD

- The Content-Type header tells the Web Server how to read the "body" of your request.
    - `application/x-www-form-urlencoded` This is the standard "language" of web forms.
    
    - It tells the server: "I am sending you data that looks like key1=value1&key2=value2."



## BRUTE FORCE

- Now , brute force is trying the differnt usernname from the previous section with different passwords.

- So, here too i use `fuff`

- So , we had saved username.txt in the previous task and now we will have to move to the folder with that .txt file in order to brute force .

- Command: `fuff -w usernmae.txt:W1,pass_path.pass.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.49.149.197/customers/login -fc 200`

    - Since we are inputting username and password, instead of FUZZ we use W1 and W2.
    - `-fc` is used to filter code, and we filter 200 OK because if pass is not found then server returns `NOT FOUND` Which is `200OK`, so we filter it 



## LOGIC FLAW

- This is the condition when the authentication code isnt well written , which can lead to  bypass of the authentication 

- A logic flaw occurs when an application makes authorization decisions based on request properties (such as URLs or parameters) instead of consistently validating the user’s identity and permissions.

- So I studied 2 kinds here :

#### Logic Flaw Example

- So here, i was given a code ( a vuln code in php ) and i learnt why is this a risk.
- The code is as follows:

if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}

- So its in PHP and in PHP `===` means comparison, ie `admin` should be equal to `admin` , only then it will go to if condition

- These are just small reason for logic flaw, but majorily is due server not checking the user authorization and only verifying when /admin is in URL

- Now, this is flaw, because , see the `(url.substr(0.6)==='/admin')` it only alows users if ID == admin, else it send the user to else part ( webserver)

- `Security decisions must be based on WHO the user is, not WHAT the URL looks like.`
- The Building Setup

    - There is a building, Inside are:
        - A café (public)
        - An admin room (restricted)

    - There is ONE rule:
        - Only admins may enter the admin room

- How the system is wrongly designed
- There is a bouncer, but he behaves incorrectly.
- The bouncer does not always check IDs.

- Instead, he follows this rule:

    - “I will only check someone’s ID if they say the exact word admin.”
    
    - This is the logic flaw.

- Normal user flow
    - A normal user walks in and says:
    - “I want to go to the café”

    - The bouncer says:
    - “Okay, no ID needed”

- User goes to café (correct behavior)

- Hacker flow (the bypass)

- The hacker walks in and says:
    -  “I want to go to adMin”

    - The bouncer thinks:
    - “That’s not exactly admin, so I won’t check ID”

- The bouncer lets the hacker walk past without checking anything
    - Inside the building, the hallway still leads to the admin room
    - The hacker enters the admin room without ever showing an ID

- To resolve it , we can write the same code as follows : 

- $lowercase_url = strtolower(url.substr(0,6));

if($lowercase_url === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}

- So , what it does is check is all the ID is in lowercase before letting in , even the server check not only the guard

- So now, the hacker says `aDmin` , bounce be like bro u mean `admin` ik , im not dumb and yeah lemme check u in if part and boom bro does not have a cookie in if and bouncer then kicks him out of both if and else part


### Logic Flaw Practical

- So, here my goal was to get reset email to my acc for resetting roberts password.
- So , this is pure ` GET , POST , REQUEST ` METHOD manipulation

- So for this i used `curl` command.
- Here , the developer's mistake was using `REQUEST` for PHP    METHOD instead of `POST`
- Maybe due to laziness or to skip a level of thinking 
- SO now, ` curl 'http://10.48.172.55/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'` , here i use curl to talk to webiste, this is to check if email is sent to robert or no !, just random checking.
- In email, we used `%` instead of `@` because, URL sometimes confuses itself for `@` as username and might give error.

- Now, the important part :
    - I create an account for me with email as `meow@customer.acmeitsupport.thm` this is my email

    - Now, i use command: `curl 'http://10.48.172.55/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=meow@customer.acmeitsupport.thm'`

    - So, this will give us link to reset pass to our email than robert's
    - why? because in PHP if Developer uses `REQUEST` METHOD, then more priority will be given to `POST` than `GET`
    - In our case, the URL uses `GET` method , but for data , ie `POST`method, i wrote my email, and hence server sends the email to be because its in POST part .


## COOKIE TAMPERING

- Cookies contain our status, ie if we are logged in or no and what privilage do we have
- its not in plain text , the data in cookies will be hashed

- command: `curl -H "Cookie: logged_in=true; admin=true" http://10.49.173.23/cookie-test `
- This will make our status to true adn admin to true as well, hence logging us in as admin !