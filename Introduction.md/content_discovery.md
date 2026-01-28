    # CONTENT DISCOVERY

## MISSION

- Learn about content discovery of a website.
- content -> files,dir,pages etc which a user is not allowed to see
- Learn about -> MANUAL , AUTOMATED and OSINT methods


## PROGRESS


### MANUAL 

- `robots.txt` -> These are used in URL section as an endpoint and it gives us if there are any sites the user is not allowes to access

                ex: https://website_name/robots.txt

- `favicon` -> sometimes , frameworks are used to build websites and in page source , we might have a link to favicon image of the website , which wasnt removed by the user . 

                Now , using a commands, we can convert this image into md5sum and then find out the details about the framewok used via the hash value ( comparing the hash value in the following link : [https://wiki.owasp.org/index.php/OWASP_favicon_database])

                command : 
                # curl webfavicon/images/favicon.ico | md5sum


- `sitemap.xml` -> These are used in URL section as an endpoint and it gives us  ALL sites the owner wants us to have a look at  and also sites in development or old sites 

                    ex: https://website_name / sitemap.xml



- `HTTP Headers ` -> This is very dangerous sometimes , cause it might reveal webserver details.

                    Curl is used to get heep header as follows :

                    command :
                    # curl https://website_name -v 
                    where -v is verbose


- `Framework stack ` -> To get more details on framework stack, we can try locatiing to that stack and see if there are any important information



## OSINT ( OPEN SOURCE INTELLIGENCE)

- This utilized few available websittes and techniques like google dorking to get us some more information

- `Google Dorking ` -> This is nothing but utilizing special search methods to get filtered results . You have to write this in google's search bar or url bar :

    - `site` -> `site:tryhackme.com` ->returns results only from the specified website address

    - `inurl` -> `inurl:admin` -> returns results that have the specified word in the URL

    - `filetype` -> `filetype:pdf` -> returns results which are a particular file extension

    - `intitle` -> `intitle:admin` -> returns results that contain the specified word in the title


- ` WAPPALYZER ` -> Wappalyzer [https://www.wappalyzer.com/] is an online tool and browser extension that helps identify what technologies a website uses

- `WAYBACK` -> This gives us all  old websites that were available before this current websites existed [https://archive.org/web/]

- `GIT` -> Its a `version control system` which is used to track changes in projects . Used in team projects . Git hub's search feature can be used to get informtion about the website, such as dir , source code etc


- `S3` -> These are storage cloud buckets used in ` AMAZON AWS `. S3 Stands for ` simple storage service ` and The format of the S3 buckets is `http(s)://{name}.s3.amazonaws.com` where {name} is decided by the owner


## AUTOMATED DISCCOVER 

- This is nothing but using tools such as dirb(directory buster) , FUZZ faster you fool ( ffuf ), gobuster ( go -> language via which this tool was created and buster , yk bust all the information)

- `ffuf` -> pwerful fuzzing and filter .

            COMMAND :
            ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://MACHINE_IP/FUZZ

- `dirb` -> basic, legal , discovery

            COMMAND :
            dirb http://MACHINE_IP/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt

    
- `gobuster` -> quick directory brute force ( fast )

            COMMAND:
            gobuster dir --url http://MACHINE_IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt