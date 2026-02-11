# RACE CONDITION

## INTRODUCTION 

- `program` --> this is piece of code for performing a task ( static) // analogy : Learn the recepie to make coffee
- `process` --> executing the code  and processing the task ( dymanic) // analogy : preparing the coffee
- `thread` --> smallest unit of execution // analogy : you along making biriyani
- `multi-threading` --> multiple threads workiing on one or many programs // analogy : me cooking chicken, mom making rice


## RACE CONDITION ?

- So, is we have 2 threads working on same shared data without synchrnization, race conditions occur.
- So what is this?
    - Lets say we have 100$ in acc.
    - thread one withdraw 20$ 
    - simulatenously thread 2 withdraw 30$
    - so originally toatl 50$ has to be withdrawn , but because there was no synchronization, thread2 will update the balance as 70$, ignoring the 20$
    - This is race condition.

- EXAMPLE 2:
    - Lets say we have 50$
    - thread 1 withdrew 40$
    - it takes time to update the balance
    - simulatneously thread2 withdrew 40$, as thread1 had not updated the bal.
    - tho , bal was just 50$ , totally 80$ were withrew and still the bal was 10$


## WEB-APPLICATION ARCHITECTURE

### CLIENT - SERVER MODEL

- CLIENT --> SOMEONE WHO REQUEST 
- SERVER --> BACKEND PROCESSING THE CLIENT REQUEST

- WEB-APPLICATOIN HAS 3 TIER:
- PRESENTATION TIER --> HTML/CSS/FRONTEND
- APPLICATION TIER --> BACKEND
- DATA TIER --> DATABASE

### STATES 

- From where user input --> reply the server gives
- All this is carried out in various states

- EXAMPLE:
    - Now lets say i have to transfer money to frnd acc
    - so i enter amount --> click transfer ( frontend )
    - in the backend, the server ask the DATABASE, if i have entered money in my account.
    - then , if availabel, it shows a success else failure with why !
    - so states are --> success / failure
    - Now this is simple state, there might be complex business logic as well.


## EXPLOITING RACE CONDITION

- This was bit complicating.
- i was given 2 accounts, and min bal of 7$ each ,now using race condition i had to get 100$ in any one of the account !
- Here i first turned on proxy-foxy.
- Then , i turned on burp-suite.
- after this, i was in the proxy tab and there was POST request for transaction
- i right clicked and sent it to the repeater.
- After sending it to repeater, i create a grp tab and created 20 duplicate
- Then , i had 3 option in send :
    - `single-connection` --> this sends packet one by one , with delay
    - `multiple-connection` --> this create a tcp-connection, have few multiple packets in it and then send it ( have delays , but quite faster)
    - `parallel` --> this has `last-byte` magic, ie I used `http/2`'s `multiplexing`, meaning multiple data are first sent in pipe and the last byte is not sent, yes, it is not sent and when all request come at a single start point , they are executed at same time, hence have least time-delay

- The mistake i was making was , using http/1.1, this doesnt work when using `parallel` .


## DETECTION AND MITIGATION

- DETECTION :
    - Normally, this cannot be detected, unless checked via LOGS, where we might find same user , doing multple request in milli-second gap and redeeming the price twice .

- MITIGATION :
    - SYNCHRONIZATIOIN MECHANISM
    - ATOMIC OPERATIONS
    - DATABASE OPERATIONS ( ACID PROPERTIES)