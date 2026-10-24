# Write-up: Blind SQL injection with time delays and information retrieval
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval  
Difficulty: Practitioner   
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives 
Retrieve the password of `administrator` user and log into their account    

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

### Using `CASE`
`CASE` can execute different expressions depending on whether a condition is true or false. The general structure is:
```text
CASE WHEN condition THEN value1 ELSE value2 END
```

### `LENGTH()`  
`LENGTH()` returns the number of characters in a string.  

### `SUBSTRING()`  
`SUBSTRING()` extracts a specific part of a string using the following syntax:  
SUBSTRING(string, start, length)    
For example: `SUBSTRING(password,1,1)`  
`password`: the string to extract characters from.  
`1` (second parameter): the starting position of the character to extract. Positions start from 1.  
`1` (third parameter): the number of characters to extract.   

### [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)
Use it to get the payload for each database management system   

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
So I used `pg_sleep(10)` to attack.  

### 3. Confirm that `users` table and `administrator` user exist   
I injected:  
```text
'||(SELECT CASE WHEN(username='administrator') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)--
```
The response slept 10 seconds, so `users` table and `administrator` user exist    

### 4. Identify the password length  
I injected 2 payloads: 
```text
'||(SELECT CASE WHEN(username='administrator' AND LENGTH(password)>1) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)--
```
The response slept 10 seconds   
```text
'||(SELECT CASE WHEN(username='administrator' AND LENGTH(password)>30) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)--
```
This time, it did not sleep at all  
Therefore, `1<the password length<=30`

Now, I sent the request to Intruder  
In the tab Burp Intruder, I selected the number used in `LENGTH(password)>§1§` as the payload position and clicked **Add §**  
I chose **Sniper attack** because there was only 1 payload position  
For the payload configuration:  
Payload type: `Numbers`  
From `1`  
To `30`  
Step `1`  
![Payload in Burp Intruder](img/09-payload.png)   
After starting attack, the response showed that the response got `response received>10000` for values from `1` to `19`, while the `response received` for `20` was just   
This means: `LENGTH(password)=20`  
![Intruder attack: the length of password](img/10-intruder.png)  

### 5. Retrieve the administrator password  
I used the following payload to test the first character of the password:     
```text
' AND (SELECT SUBSTRING(password,1,1) from users WHERE username='administrator')='a'--
```
I marked the character position and the character as two payload positions: `SUBSTRING(password,§1§,1)='§a§'`  
I used **Cluster bomb attack** because there are two independent values to test:  
The first position represents the character position in the password.  
The second position represents the character being tested.  
For the first payload position, I used:  
Payload type: `Numbers`  
From `1`  
To `20`  
Step `1`  
![Payload for number in Burp Intruder](img/11-number.png) 
For the second payload position, I used Brute forcer to test possible characters.  
![Payload for character in Burp Intruder](img/12-character.png)  
In Settings -> **Grep Match**, I added `Welcome back!`   
After starting the attack, I recorded the character that produced **Welcome back!** for each password position.  
![Intruder attack for password](img/13-password.png)  
The administrator password was: `il2mtulhg7q5dub2kqc4`  
And the lab was solved:  
![Lab solved](img/14-lab-solved.png)  
