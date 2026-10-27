# Write-up: User ID controlled by request parameter, with unpredictable user IDs  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids  
Difficulty: Apprentice  
Status: Solved  

## Lab description:   
![Lab description](img/02-lab-description.png)  

## Objectives  
Obtain the API key for the user `carlos` 

## Background  
### Broken Access Control  
Broken Access Control occurs when an application fails to enforce proper restrictions on what authenticated users are allowed to do.  
### IDOR (Insecure Direct Object Reference)
IDOR occurs when an application uses a user-controlled identifier to access an object but fails to verify whether the current user is authorized to access that object.  
An attacker may exploit this vulnerability by changing an identifier in a request to access another user's data.  

## Steps  
### 1. Log into `wiener` account  
I logged in with the information provided by the lab:  
Username: `wiener`  
Password: `peter`  
The account was a normal user  
I intercepted HTTP request and sent it to Repeater in Burp Suite.  
![Request of Wiener](img/03-request1.png)  

### 2. Obtain `id` of `carlos` user  
I returned to the home page and browsed the posts until I found a post authored by `carlos`  
![Carlos's post](img/04-post.png)  
I clicked the author's name and inspected the resulting HTTP request in Burp Suite to identify the UUID associated with `carlos`.    
![Request of Carlos](img/05-request2.png)  

### 3. Obtain API Key of `carlos` user  
I changed the `id` parameter from `565e0942-0a22-4982-9bab-2b85b6612246` to `e1b1f68f-3fae-45c5-90a4-b5bd16d36ea0` (`wiener` to `carlos`)    
I sent the modified request and inspected the response.   
In the response, API Key of `carlos` user was revealed: `kXtd78B9kVeVHA6xwBmX4WuNyTzwqfqj`
![Response](img/06-response.png)  
I returned to the lab page and submitted the retrieved API Key 

And the lab was solved.  
![Lab solved](img/07-lab-solved.png)


