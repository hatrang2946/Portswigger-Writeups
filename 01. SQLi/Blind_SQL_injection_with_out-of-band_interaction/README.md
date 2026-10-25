# Write-up: Blind SQL injection with out-of-band interaction   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band  
Difficulty: Practitioner   
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives 
Cause the lab to delay 10 seconds  

## Background  
### Blind SQL injection  
**Blind SQL injection** occurs when an application is vulnerable to SQL injection, but the results of the injected query are not directly displayed in the response. Instead, information must be inferred from differences in the application's behavior.  
In this lab, the application does not reveal query results or display different responses based on the query outcome. Instead, the vulnerability can be exploited by introducing a time delay and measuring the response time.  

### Time-based blind SQL injection  
Time-based blind SQL injection is a technique that injected SQL injection payload and observe how long the application responds.  
If an injected SQL expression causes a delay of 10 seconds, the HTTP response will also be delayed about 10 seconds  

### String concatenation  
String concatenation combines multiple strings into one.  
For example: `'abc' || 'def'` produces `abcdef`  

### [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)
Use **String concatenation** and **Time delays** to attack this lab   

## Steps  
### 1. Establish a baseline  
Firstly, I chose **Gifts** category  
In Burp Suite, I intercepted this request and sent it to Repeater
![Request](img/03-request.png)
The important parameter in this lab is the `TrackingId` cookie  

### 2. Test payload  
I tested payloads using different database-specific time-delay techniques and string concatenation syntax  
![Payload 1](img/04-payload1.png)  
![Payload 2](img/05-payload2.png)  
![Payload 3](img/06-payload3.png)  
![Payload 4](img/07-payload4.png)  
The third payload caused a noticeable delay, with a response time of 10,237 milliseconds.  
![Time delay](img/08-time-delay.png)    
And the lab was solved  
![Lab solved](img/09-lab-solved.png)  

