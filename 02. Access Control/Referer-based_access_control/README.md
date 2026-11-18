# Write-up: Referer-based access control  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-referer-based-access-control    
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
### Privilege escalation  
**Vertical privilege escalation:** A lower-privileged user accesses functions reserved for higher-privileged accounts (such as an administrator)  
**Horizontal privilege escalation:** A user accesses resources or data belonging to another user with the same privilege level.  
In this lab, it belongs to **vertical privilege escalation**
### Referer-based access control  
Referer-based access control is a security mechanism where an application relies on the HTTP Referer header to determine whether a user is authorized to perform a sensitive action or access a restricted endpoint.    
### Cookies  
Cookies are small pieces of data stored by the browser and sent back to the server with every HTTP request. In web applications, session cookies are commonly used to maintain user state and identify who is making the request after authentication.  

## Steps  
### 1. Establish a baseline  
Firstly, I signed in with **administrator** account:  
`Username: administrator`  
`Password: admin`  
I accessed **Admin panel** and chose user `wiener(NORMAL)` to upgrade user  
When I signed in with **wiener** account, the lab was not solved  
Then, I returned **administrator** account and downgrade **wiener** user and upgrade **carlos** user  
In Burp Suite, I intercepted this GET request and sent it to Burp Repeater  
![Request](img/03-request.png)  
#### Reason why this lab was not solved  
Upgrading `wiener` directly from the `administrator` account is just normal administrative functionality, not an exploit. To solve the lab, the privilege escalation request must be triggered from an unprivileged session (`wiener`) by bypassing the server's access control.

### 2. Privilege escalation using method-based access control  
In Burp Suite, I came to sign-in request of **wiener** user  
I copied cookie session of **wiener** user   
![Wiener's cookie session](img/04-request2.png)  
In the GET request, I changed the value of administrator's session cookie with cookie session of **wiener** user and the `username` parameter in the request body from `carlos` to `wiener`      
![GET request](img/05-request3.png)
I sent the modified request.  
I returned to the lab page and the lab was solved    
![Lab solved](img/06-lab-solved.png)
