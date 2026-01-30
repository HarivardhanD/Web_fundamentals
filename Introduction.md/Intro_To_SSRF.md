# Intro to SSRF

- SSRF -> SERVER SIDE REQUEST FORGERY.

- SSRF is when an attacker tricks a server into making requests to places the attacker is not allowed to access directly.

- This , will lead to access to tokens, database, server resources.

- There are 2 types :
    - Regular SSRF: The attacker sees the response (page content, data, error message)

    - Blind SSRF: The attacker does not see the response, but can still confirm it happened


- Sometimes, what happens is after we enter the ATTACKER URL, then what happens is sometimes extra words or letters are added at the end of the attacker URL and this causes error, hence to avoid this we add `&x=` at the end of our URL.
    - eg: https://website.thm/item/2?server=attacker.url`&x=` 

- Now, I had commited a mistake , ie URL in URL. The mistake:
    - `https://website.thm/item/2?server=https://server.website.thm/flag?id=9&x=`
    - ie as we know , https is start of url and `?` is start of query parameter .
    - Now after `?` i again used https but the problem was `?` near the flag,which was another query parameter and this causes confusion, hence leading to error 

        - If you put a URL inside a query parameter, you MUST URL-encode it.
        - Why? Because these characters are special in URLs:
            - ? → starts query

            - & → separates parameters

            - = → assigns values

            - If they appear unencoded inside a value, the server misreads the URL.
        - Server thinks:
            - server = https://server.website.thm/flag

            - id = 9

            - x =

        -  Wrong parsing → wrong result

    - Fixed (encoded)
        - https://website.thm/item/2?server=https%3A%2F%2Fserver.website.thm%2Fflag%3Fid%3D9&x=


        - Server thinks:

            - server = https://server.website.thm/flag?id=9

            - x =

        - Correct parsing → correct result

    
- Common Encodings :
Character	Encoded
- `:`   --->   %3A
- `/` ---> 	 %2F
- `?` --->  	 %3F
- `=` ---> 	 %3D
- `&` ---> 	 %26



## FINDING AN SSRF

- Easy to fini few SSRF such as :

    - When a full URL is used in a parameter in the address bar:
        - EX: https://website.thm/form?`server=server.website.thm/flag`

    - A hidden field in a form:
        - HTML form: EX:  <input type="hidden" name="server `value="internal-api"`>

    - A partial URL such as just the hostname:
        - EX: https://website.thm/form?server`=api`

    - only the path of the URL:
        - EX:  https://website.thm/form?dns`=/forms/contact`



## DEFEATING COMMON SSRF DEFENSES

- So, there are three ways for protection :

1. DENY LIST

- In this,we are having a list which has IP address such as local host -> 127.0.0.1. whihc is on list
- Hence, since this ip is on list, user cant access it.

- n a cloud environment, it would be beneficial to block access to the IP address 169.254.169.254, which contains metadata for the deployed cloud server, including possibly sensitive information

- This metadata contains sensitive informatioin in cloud environment
- Why this localhost specifically is being protected ?
- This localhost, is allowing server to talk to itself and its not over internet.
- Since its not over internet, the server things it is safely talking to itself, ie givin it tokens, http headers , important credentials etc.
- Now , this can be bypassed, because this localhost IP , can be written in multiple differnt ways, hence the attacker can use different way to access the localhost and talk to server using SSRF

2. ALLOW LIST

- “Block everything unless it matches this safe pattern.”
- Like when it sees, a website is not starting from `https`,rather `http`, it blocks the website.
- But bypass is like : https://website.thm.attacker.com
    - In the above link, the website started with https and then attacker put his url attached to the web url, but that wont be checked, because `https` condition is already true


3. REDIRECT LIST

- A trusted page redirects to another URL.

- EX: `https://website.thm/link?url=http://evil.com`



## SSRF PRACTICAL

- So, here i was given a website and there is flag in the endpoint ie in the `/private`

- So, now i had to get the flag present in this endpoint.

- So, what i did was first, in the create_account section we had a new option to choose AVATAR

- So i clicked the avatar and went to view page source

- Now, how did u know if i there is SSRF ? Here is how :
    - First of all, there is private which server can fetch .
    - Then when i view page source, i saw the follwoing:
        - `<input type="radio" value="avatars/cat.png">`
            - This is a huge hint Because:

                - That value looks like a file path

                - It is sent to the server

                - Server likely uses it directly

                - At this moment, the thought should be:

- “If I change this path, will the server fetch something else That is SSRF thinking.


    - See, the `value` had a path , so then i tried changing it to `value=/private`
    - But, couldnt open LOL
    - Now, it returned an error saying u cant access it, so the filter might be for word matching
    - So, what i did was , in the value field , tried direcotry traversal
    - `value = x/../private`

    - Hence, i could upload the avatar and again went to page source, got the flag.
    
    
# --------------------------------------------------------------------------------------------------------------------------------------------
# CHEAT SHEET - SSRF
# --------------------------------------------------------------------------------------------------------------------------------------------

## 1. What is SSRF ?
    - Server makes requests attacker can’t
    - Attacker controls destination

## 2. Why dangerous?
    - Server can access:
        - Internal APIs
        - private endpoiints
        - Cloud metadata 169.254.169.254
        - Tokens / configs

## 3. Types

    - Regular → response visible

    - Blind → response hidden

## 4. How to spot SSRF

    - Look for:
        - URL / path in parameter
        - Hidden form fields
        - Image / avatar / fetch features
        - Server returns base64
        - Internal endpoint exists

## 5. Lab pattern (avatar case)

    - User input → Server fetch → Server returns
    - Path in HTML "value"
    - Modify "value"
    - Server fetches internal resource

## 6. Exploit flow

    - /private        → blocked
    - x/../private   → allowed

    - String filter
    - Path resolution bypass

## 7️. Common payloads

- /private
- x/../private
- http://127.0.0.1
- http://localhost
- 169.254.169.254

## 8️. Base64

- `echo "BASE64" | base64 -d`

## 9️ SSRF defenses & bypass

- Deny list
    - Blocks: 127.0.0.1, /private

    - Bypass:

        - x/../private
        - 127.1
        - localhost.
        - 2130706433

- Allow list

    - Checks prefix only

    - Bypass:

        - `https://trusted.com.attacker.com`

- Redirect trust

    - Trusted URL redirects internally


## 10 URL inside URL

- Encode `? & =`

- Use:
    - `&x=` to prevent addition of words after the attacker adds his URL


# If user input controls what the server fetches → SSRF.