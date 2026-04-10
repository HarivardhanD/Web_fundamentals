# OWASP API SECURITY TOP 10

- OWASP -> OPEN WORLD APPLICATION SECURITY PROJECT
#

## UNDERSTADING API AS FRESHERS

- API stands for Application Programming Interface. 

- API is a middleware that facilitates the communication of two software components utilising a set of protocols and definitions

- API is a building block for developing complex and enterprise-level applications.
#
## Vulnerability I - Broken Object Level Authorisation (BOLA)

- ### HOW DOES IT HAPPEN ?
    - BOLA is nothing but IDOR [ INSECURE DIRECT OBJECT REFERENCE]
    - This happens when  user uses the input functionality and gets access to the resources they are not authorised to access. 

- ## IMPACT
    - This can lead to data leakage
    - In some cases complete account take over


#

## VULN 2 --> BROKEN USER AUTHENTICATION ( BUA )

- Broken User Authentication (BUA) reflects a scenario where an API endpoint allows an attacker to access a database or acquire a higher privilege than the existing one.
- The primary reason behind BUA is either invalid implementation of authentication like using incorrect email/password queries etc., or the absence of security mechanisms like authorisation headers, tokens etc.

- This is like ur pass is weak or there are no tokens for protecting your password or the password is in plain text and not hashed or it is accessible via GET / POST requests

- MITIGATIONS :
    
    - Strong password policies
    - Multi-factor authentication (MFA)
    - Secure token handling
    - Login rate limiting 


#

## Vulnerability III - Excessive Data Exposure


- ### HOW DOES IT HAPPENS ?

    - Excessive data exposure occurs when applications tend to disclose more than desired information to the user through an API response.

    - A malicious actor can successfully sniff the traffic and easily access confidential data, including personal details, such as account numbers, phone numbers, access tokens and much more

- ### MITITGATION 
    - Ensure time-to-time review of the response from the API to guarantee it returns only legitimate data and checks if it poses any security issue.   
    - Avoid using generic methods such as to_string() and to_json(). 
    - Use API endpoint testing through various test cases and verify through automated and manual tests if the API leaks additional data.


#



## Vulnerability IV - Lack of Resources & Rate Limiting

- LACK OF RESOURCES & RATE LIMITING means  APIs do not enforce any restriction on the frequency of clients' requested resources or the files' size.

- This can lead to DOS attack or non-availability of the service

- This is why its better to keep gap between sending otp or also we can use captcha so that scripts cant be used for automating such attacks and bots to record such sus attack 


#

## VULNERABILITY V - Broken Function Level Authorisation 

- This is same as IDOR , but here the user is authorized as higher privilaged user / admin and can perform acts

- So lets  undetstand this with an example:
    - We have a website and the website has admin page and user page 
    - So only a person with `ADMIN=1` and with correct `autorization key` access the admin page

    - Now alice used her own authroization key and wrote ` isadmin=1` and even tho she wasnt admin, she was allowed becasue the server never checked the db if she is admin or no and based on her input gave her the access , which is a flaw.


#

## VULNERABILITY VI - MASS ASSIGNMENT


- `MASS ASSIGNMENT` - It is when the server automatically saves  all user input into the database without validating the user input

    - EXAMPLE :
        -  API expects: name, username, password
        - Attacker sends:
            - name=John
            - username=john123
            - password=abc
            - credit=100000   ❌ (not allowed but accepted)

        - Server saves it → user gets extra credit [ server does not check if credit can be modified in db or no and directly do what the user input mentioned]


- MITIGATION :
    - Allow only required fields (allowlist)
    - Ignore everything else


#

## Vulnerability VII - Security misconfiguration

- Incorrect and poorly configured security control

- Like showing sensitive details in error messages , publicly accessible cloud storage etc

- . API documentation, a list of endpoints, error logs etc., must not be publically accessible to ensure safety against security misconfigurations. 


- MITIGATION :
    - Remove unnecessary pieces of code snippets, error logs etc. and turn off debugging while the code is in production.
    - Disable directory listing
    - Disable default usernames and passwords for public-facing devices (routers, Web Application Firewall etc.).



#

## VULNERABILITY VIII - INJECTION


- Injection flaws occur when user input is not filtered and is directly processed by an API

- Injection flaws may lead to information disclosure, data loss, DoS, and complete account takeover.


#


## VULNERABILITY IX - IMPROPER ASSET MANAGEMENT

- Lets learn this with an example:
    - We have 2 API , one is new and other is old
    - new is having good security , but old is outdated now, but still it has some links with the new API
    - So now attacker can attack new API using the flaws present in the old One OR they can get more information regarding the target using the old api 

- So its necessary to remove old API or keep it updated 


- MITIGATION :
    - API for production , Q&A etc must be kept segregated
    - Also remove unused API 
    - Keep documentation of all API aspects being used


#

## VULNERABILITY X - Insufficient logging and monitoring

- Attacker conduct malicious activity on ur server --> but u cant track because `, there is not enough evidence available due to the absence of logging and monitoring mechanisms`

- MITIGATION :
    - Along with Network event / server logging , use API-Logging for identifying thr IP_Address, end points viewed, timeStamp etc

    - Implement custom event to alert sus activities
