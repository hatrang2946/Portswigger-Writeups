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
![Payload for username](img/04-payload-username.png)
After sending attack, the length of the response with `username=ao` was 3354, while the others were 3352  
Therefore, right username was `ao`  
![Right username](img/05-username.pg)  

### 3. Find correct password in the list   
I clicked `clear §` on `username=§test§` and replaced `test` with `ao`  
Similar as finding username, I added § for `password=§123§`, used **Sniper attack**, cleared the old list and pasted the list of **candidate passwords**  
![Payload for username](img/06-payload-password.png)  
After sending attack, the response with `password=123123` got status **302**, while the others got status **200**  
![Right password](img/07-password.pg)
Therefore, right password was `123123`  

### 4. Log in  
I returned the lab page and used username and password which were found to log in  
And the lab was solved  
![Lab solved](img/08-lab-solved.png)



