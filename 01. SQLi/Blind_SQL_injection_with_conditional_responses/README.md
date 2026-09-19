# Write-up: Blind SQL injection with conditional responses   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses  
Difficulty: Practitioner   
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives 
Retrieve the password of `administrator` user and log into their account  

## Background  
### Brute-force attack  

### [Burp Intruder](https://portswigger.net/burp/documentation/desktop/tools/intruder)

### Substring


## Steps  
### 1. Establish a baseline  
Firstly, I chose the **Pets** category
In Burp Suite, I intercepted this request and sent it to Repeater  
![Request](img/03-request.png)   

### 2. Confirm the parameter is vulnerable to blind SQLi  
With the value of the **trackingId** available in the request, I injected these payloads one by one: `' AND 1=1--` and `' AND 1=2--`  
Although both queries returned status `200 OK`, **Welcome back!** message just appeared with the first payload. This indicated that  

### 3. Confirm that users table and administrator user exist  
I injected these query respectively:    
```text
' AND (SELECT 'x' from users LIMIT 1)='x'--
```
```text
' AND (SELECT 'x' from users WHERE username='administrator')='x'--
```
Both responses returned **Welcome back!** message, so `user` table and `administrator` user existed  
Why add limit 1  
 
### 4. Retrieve the administrator password  
#### Identify the length of password  
I injected 2 payloads: 
```text
' AND (SELECT 'x' from users WHERE username='administrator' AND LENGTH(password)>1)='x'--
```
```text
' AND (SELECT 'x' from users WHERE username='administrator' AND LENGTH(password)>30)='x'--
```
The first payload returned successfull response, while the second one did not. This indicated that the length of password was within in range (1,30)  
Now, I sent the request to Intruder  
In the tab Burp Intruder, I chose the number used to compare with the length of password and click **Add §**  
I sent with the payload type: `Numbers`, Number range From `1` To `30` Step `1`  
![Payload in Burp Intruder](img/04-payload.png)   
In Settings, **Auto-pause attack**, I chose **Pause if an expression on the list is missing from a response** and I add `Welcome back!` message.  
![Auto-pause attack message](img/05-message.png)
Start attack: 
