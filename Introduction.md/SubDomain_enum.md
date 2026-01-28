# Subdomain Enumeration

 
## BRIEF

- SubDomain enumeration is the process of findoing the subDomains of a domain , trying to find more vulnerabilities.

- Three methods, gonna learn today for the same and they are :
    - OSINT ( OPEN SOURCE INTELLIGENCE)
    - Brute Force
    - Virtual Host


##
## OSINT - SSL/TLS CERTIFICATE

- SSL/TLS certificates are created for a domain to protect it over HTTP
- These certificates are created by CA
- Now , CA creates Transperency logs for these , which is publicly available
- These logs keep a track of certicicates by CA which were bi-mistakenly created by CA and also has all the details about a domain and its subdomain

- The link for the same is [https://crt.sh/]


## OSINT SEARCH ENGINES

- Here we use advances google search for finding subdomains .
- The SEARCH: `site:*.domain_name.com -site:domain_name.com `
    - This gives us subdomain name and '-' with site removes the domain


## DNS BRUTE FORCE

- BruteForce DNS is method of trying multiple ie millions or billions of SubDomains for a particular domain and this is done using tools.

- In our case, we used the tool : `dnsrecon`

- The command is as folloes : `dnsrecon -t brt -d acmeitsupport.thm`


## OSINT -SubList3r

- OSINT has smth called ` subList3r` which is  a python code used to find all subdomains

- Its OSINT tool and command is : ` ./sublist3r.py -d acmeitsupport.thm `


## VIRTUAL HOST

- Here we use `FFUF` tool
- So the command: `ffuf -w wordlist.txt -H "Host: FUZZ.domain_name.com" -u url_target -fs {size}`
    - Here :
    - -w is used to give the path where we have set of wordlist
    - -H is used to define host header or edit host header
    - As u see in Host: we have FUZZ written and then .domain.com
    - So here, in place of FUZZ , everyother subdomain are added followed by domain.com
    - -u is used to deifine url of target
    - -fs is used to filter based of size


## SUMMARIZING

- So the difference btw subList3r, dnsrecon and fuff is :
    - sublist3r is OSINT tool and its like googling about the domain and asking browsers about the information regarding subdomain

    - dnsrecon is asking information about the subdomain to the DNS
        - Its like, going to building and seeing the available offices on the Building Board

    - fuff is asking the webserver directly about the subdomain
        - Some offices might not be registered yet, so here we ask guard where is this specifci building and then guard gives us the solution

        - In fuff what happens is , since it ask webserver, webserver if subdomain not availbale cannot say no, so it gives notfound or welcome page, so this always gives us success with repeated webpages or answrs and for filtering this we use filtersize