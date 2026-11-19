# Write-up: 2FA simple bypass  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass  
Difficulty: Apprentice  
Status: Solved  

## Lab description:   
![Lab description](img/02-lab-description.png)  

## Objectives  
Access `carlos` account without 2FA vertification code   

## Background  
### Reason why this lab can be solved  
The application does not properly enforce the 2FA verification step before granting access to the account.  

## Steps  
### 1. Log in with `wiener` account  
I used provided information to log in:  
`username: wiener`  
`password: peter`   
After logging in, I clicked **Email client** button and 2FA verification code was retrieved   
![Wiener's 2FA verification code](img/03-code.png)  
I entered the code and successfully logged in as `wiener`  
I then inspected the account page URL, which was: `/my-account?id=wiener`   
![Wiener's link](img/04-link.png)  

### 2. Log in with `carlos` account  
Similar as Step 1  
I used provided information to log in:  
`username: carlos`  
`password: montoya`  
After logging in, I clicked **Email client** button and 2FA verification code was retrieved   
![Carlos's 2FA verification code](img/05-code2.png)  
I entered the retrieved 2FA verification code but I was unable to access `carlos` account    
#### Reason  
Although I had submitted the correct username, password, and 2FA verification code, the application did not grant access to the account.  
This indicated that the account page might be accessible without completing the 2FA verification step.  

### 3. Bypass 2FA verification code  
I changed the link by replacing `/login2` with `/my-account?id=carlos`  
Instead of completing the 2FA verification process through `/login2`, I accessed the account page directly.  
![Carlos's link](img/06-link2.png)  
I entered the link and I could access with `carlos` account  
And the lab was solved   
![Lab-solved](img/07-lab-solved.png)
