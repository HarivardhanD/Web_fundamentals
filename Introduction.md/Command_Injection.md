# COMMAND INJECTION

## 1. What is command injection?

-  Command injection is the abuse of an application's behaviour to execute commands on the operating system, using the same privileges that the application on a device is running with. For example, achieving command injection on a web server running as a user named joe will execute commands under this joe user and therefore obtain any permissions that joe has.

 ## 2. WHY THIS EXIST?

 - This vulnerability exists because applications often use functions in programming languages such as PHP, Python and NodeJS to pass data to and to make system calls on the machine’s operating system. For example, taking input from a field and searching for an entry into a file which is accessible on OS.

 - Most of the time, data will be stored in DB, so command injections wont work


 ## 3. EXPLOITING COMMAND INJECTION !

 - We can determine if command injection has taken place or no based on the behavior of the application

 - There are  2 types of command Injection :
    - BLIND COMMAND INJECTION
    - VERBOSE COMMAND INJECTION

### BLIND COMMAND INJECTION

- Here when we execute the payload, we cannont see if payload is executed or no
- IN order to determine if payload is executed or no, we will need to force output 
- Forcing output can be done in the following ways:         
    - Using a `ping` command : the application will hang for x seconds in relation to how many pings you have specified.
    - `sleep` command 
    - Another method is forcing output : Use `>` redirection and then redirect the outupt into a file and open it using `cat file_name`

### VERBOSE COMMAND INJECTION

- Here the application gives u output on what is being executed, ie if i execute `whoami` applicatioin will return me the users
- Command to be used for determing can be:
    - `whoami`
    - `ping`
    - `sleep`


- Now lets go through some Linux and Windows command for the same :
    - LINUX 

        - `ping`
        - `ls`
        - `whoami`
        - `sleep`
        - `nc` --> netcat ,reverse shell
    
    - WINDWOS
        - `whoami`
        - `ping`
        - `timeout`
        - `dir`


## 4. REMEDIATION COMMAND INJECTION

- First of all stop using un-necessary and dangerous functions in code
- Make sure to perform `input sanitisation`
- Use filters for user input
- But by-passing filters are sometimes possible:
    - Now lets say , user input `quotations` are filtered
    - `quotatinos` = `" "`
    - Now this can be bypassed , by writng quotations in `hexadecimal` format instead of `" "`


## 5. PRACTICAL COMMAND INJECTION

- Here i learnt that , we can use multple command injections simulatneoulsy using `;` after each command:
    - EXAMPLE : ping ip_add; whoami ; pwd 
    - All this wil be executed together