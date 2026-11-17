# Write-up: Method-based access control can be circumvented  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented    
Difficulty: Practitioner  
Status: Solved  

## Lab description   
![Lab description](img/02-lab-description.png)  

## Objectives  
Log in `wiener` account and promote to become administrator  

## Background  
### Broken Access Control  
Broken Access Control occurs when an application fails to enforce proper restrictions on what authenticated users are allowed to do.  
### IDOR (Insecure Direct Object Reference)
IDOR occurs when an application uses a user-controlled identifier to access an object but fails to verify whether the current user is authorized to access that object.  
An attacker may exploit this vulnerability by changing an identifier in a request to access another user's data.  
### Method-based access control  

### Cookies  

### `GET` and `POST` methods

## Steps  
### 1. Establish a baseline  
Firstly, I signed in with **administrator** account:
`Username: administrator`  
`Password: admin`  
I accessed **Admin panel** and chose user `wiener(NORMAL)` to upgrade user
When I signed in with **wiener** account, the lab was not solved  
Then, I returned **administrator** account and downgrade **wiener** user and upgrade **carlos** user  
In Burp Suite, I intercepted this POST request and sent it to Burp Repeater  
![Request](img/03-request.png)
#### Reason why this lab was not solved  


### 2. Privilege escalation using method-based access control  
In Burp Suite, I came to sign-in request of **wiener** user  
I copied cookie session of **wiener** user   
![Wiener's cookie session](img/04-request2.png)
In the POST request, I changed the value of session with cookie session of **wiener** user  
Then, I right clicked on the request and chose `change method request` from `POST` to `GET`  



I returned to the lab page and the lab was solved    
![Lab solved](img/07-lab-solved.png)


