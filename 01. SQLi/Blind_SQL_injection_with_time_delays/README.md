# Write-up: Blind SQL injection with time delays   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/blind/lab-time-delays  
Difficulty: Practitioner   
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives 
Cause the lab to delay 10 seconds  

## Background  
### Blind SQL injection  
Blind SQL injection occurs when an application is vulnerable to SQL injection, but the results of the injected query are not directly displayed in the response. Instead, information must be inferred from differences in the application's behavior.  
This lab is vulnerable to time-blind technique. I need to delay the lab to overpass.  

### [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)
Use **String concatenation** and **Time delays** to attack this lab   

## Steps  
### 1. Establish a baseline  
Firstly, I chose **Gifts** category
In Burp Suite, I intercepted this request and sent it to Repeater
![Request](img/03-request.png)
The important parameter in this lab is the trackingId cookie  

### 2. Test payload  
I injected each paylods for time delays and string concatenation to solve the lab  
![Payload 1](img/04-payload1.png)
![Payload 2](img/04-payload2.png)
![Payload 3](img/04-payload3.png)
![Payload 4](img/04-payload4.png)
The third payload ran with time delay 10,237millis  
![Time delay](img/05-time-delay.png)  
And the lab was solved
![Lab solved](img/06-lab-solved.png)
