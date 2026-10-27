# Write-up: User ID controlled by request parameter  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter  
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

### 2. Obtain API Key of `carlos` user  
I intercepted HTTP requet in Burp Suite.  
![Request](img/03-request.png)  
I changed the `id` parameter from `wiener` to `carlos`  
I sent the request and inspected the response.   
In the response, API Key of `carlos` user was revealed: `q6Ldh0PNKGFxHK15AsYZPyy3QUtSAxcX`
![Response](img/04-response.png)  
I returned to the lab page and submitted the retrieved API Key 

And the lab was solved.  
![Lab solved](img/05-lab-solved.png)

