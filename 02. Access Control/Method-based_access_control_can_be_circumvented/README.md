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
### URL-based access control  
Some applications implement access control based on the requested URL. For example, just users with administration can access `/admin`    
If the application does not consistently enforce access control on the URL actually processed, an attacker may bypass these restrictions.
### `X-Original-Url` header  
`X-Original-Url` is an HTTP header sometimes used by proxies or web applications to pass the original request URL.  
If an application trusts this header without properly validating it and enforcing access control, an attacker may manipulate its value to access restricted endpoints.

## Steps  
### 1. Establish a baseline  
In the lab's URL, I clicked `Admin panel` to access admin page, but my request was denied  
In Burp Suite, I intercepted this request and sent it to Burp Repeater for further testing  
![Request](img/03-request.png)

### 2. Bypass URL-based control using `X-Original-Url` header  
I changed the request path from `/admin` to `/` and I add header: `X-Original-Url /hi` after link of `Referer` parameter  
![Request with X-Original-Url header](img/04-request2.png)
After sending the modified request, I got `404 Not Found` status  
This means the application was processing the URL specified in the **X-Original-Url** header.  
Next, I changed the value of `X-Original-Url` from `/hi` to `/admin` and sent the request  
Now, the response returned the administration panel content   

### 3. Delete `carlos` user  
The response revealed a link to the user deletion endpoint.  
![Response](img/05-response.png)  
I replaced the value of `X-Original-Url` from `/admin` to `/admin/delete` and added `?username=carlos` to the request URL  
![Request to delete user](img/06-request3.png)  
#### Why add `/admin/delete` and `and ?username=carlos`   
`/admin/delete`: The endpoint responsible for deleting a user account.  
`username=carlos`: The query parameter specifying which account should be deleted.

I returned to the lab page and the lab was solved    
![Lab solved](img/07-lab-solved.png)


