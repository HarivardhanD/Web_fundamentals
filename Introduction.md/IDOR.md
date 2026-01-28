# IDOR 

## INTRODUCTION

- IDOR Stands for insecure direct object refernce

- This happens, when more trust is put of the input , ie user is trusted and no validation is done on the server side, leading to access of others data

- An example for IDOR can be : 
    - URL : http://onlineStore/id=1234  (MY DETAILS)
    - Now , i change the id to "1000"
    - When i do this , i am able to access others data and yes, this is IDOR1

- So, now inorder for normal users to not be able to understand the data , the raw data is encoded using base64 and then sent over the server.

- But can be decoded using online base64 Decoders

- Another way to protect the data is using HASHED value, which is bit more difficult to identify/decode

### TIP : If the Id cannot be detected using the above methods, an excellent method of IDOR detection is to create two accounts and swap the Id numbers between them. If you can view the other users' content using their Id number while still being logged in with a different account (or not logged in at all), you've found a valid IDOR vulnerability.

- Not all vulnerable endpoints are vivible via URL
- Sometimes, what happens is , u can see that no parameters or endpoints are being shown in url, in such case, u can go to Network Tab in developers tool and get the info or view JS file

- Now, i had to change ID but it wasnt visible in the URL
    - So , i went to developers tool -> NETWORK TAB
    - Tried to change the ID from 50 to 1 
    - Unsuccessfull
    - Then , in network tab, in the ` customer?id=50`
    - i right clicked this and chose ` copy url `
    - copy pasted URL in new tab
    - There i saw that , in URL section , i got ` customer?id=50`
    - Now , i changed the url to 1 and 3, hence got the FLAG