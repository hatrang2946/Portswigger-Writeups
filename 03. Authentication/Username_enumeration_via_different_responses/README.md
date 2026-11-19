# Write-up: Username enumeration via different responses  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses  
Difficulty: Apprentice  
Status: Solved  

## Lab description:   
![Lab description](img/02-lab-description.png)  

## Objectives  
Find correct username and password in the given list 

## Background  
### [Candidate usernames](https://portswigger.net/web-security/authentication/auth-lab-usernames)  
The list to find correct username  
### [Candidate passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)
The list to find correct password  

## Steps  
### 1. Establish a baseline  
Firstly, I clicked `My account` to log in  
`Username: test`  
`Password: 123`  
Username and Password can be filled with whatever you want  
In Burp Suite, I intercepted the POST request and sent it to Burp Repeater  
![Request](img/03-request.png)  

### 2. Find correct username in the list  
I sent the request to Burp Intruder   
I selected the value of username `username=§test§` as the payload position and clicked **Add §**  
I chose **Sniper attack** because there was only 1 payload position
For the payload configuration:  
Payload type: `Simple list`
I copy and paste the list of **candidate usernames**  
![Payload for username](img/04-username.png)



