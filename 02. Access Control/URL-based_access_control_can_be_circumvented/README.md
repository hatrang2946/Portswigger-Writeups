# Write-up: URL-based access control can be circumvented  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented    
Difficulty: Practitioner  
Status: Solved  

## Lab description   
![Lab description](img/02-lab-description.png)  

## Objectives  
Delete the user `carlos`  

## Background  
### Broken Access Control  
Broken Access Control occurs when an application fails to enforce proper restrictions on what authenticated users are allowed to do.  
### IDOR (Insecure Direct Object Reference)
IDOR occurs when an application uses a user-controlled identifier to access an object but fails to verify whether the current user is authorized to access that object.  
An attacker may exploit this vulnerability by changing an identifier in a request to access another user's data.  

## Steps  
### 1.   
On the lab page, I chose `Live chat`  
I added something into the box of `Your message:` and clicked `Send`  
I chose `View transcript` and 1 file named `2.txt` had been downloaded  
Username: `wiener`  
Password: `peter`  
The account was a normal user

### 2. Obtain API Key of `carlos` user  
I intercepted HTTP request in Burp Suite.  
![Request](img/03-request.png)  
I changed the `id` parameter from `wiener` to `carlos`  
I sent the request and inspected the response.   
In the response, API Key of `carlos` user was revealed: `mIYoIQJp1GNAnwfaOXuaRcF1oJqVSrT7`
![Response](img/04-response.png)  
I returned to the lab page and submitted the retrieved API Key  

And the lab was solved.  
![Lab solved](img/05-lab-solved.png)

