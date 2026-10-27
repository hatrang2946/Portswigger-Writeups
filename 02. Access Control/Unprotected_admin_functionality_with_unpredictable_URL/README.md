# Write-up: Unprotected admin functionality with unpredictable URL   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url  
Difficulty: Apprentice  
Status: Solved  

## Lab description:   
![Lab description](img/02-lab-description.png)  

## Objectives  
Delete the user `carlos`  

## Steps  
### 1. Discover the admin panel path  
After accessing the lab, I intercepted this request and sent it to Repeater in Burp Suite    
The response showed the admin panel path     
![Response](img/03-response.png)

### 2. Access the admin panel  
Now, I added the lab URL with `/admin-dv52x9` to access the admin panel.  
![Users](img/04-users.png)  
The panel was accessible without requiring administrator authentication.  

### 3. Delete `Carlos` user  
I clicked `Delete` option next to the user `Carlos`.

And the lab was solved.  
![Lab solved](img/05-lab-solved.png)

