# Write-up: Unprotected admin functionality  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality  
Difficulty: Apprentice  
Status: Solved  

## Lab description:   
![Lab description](img/02-lab-description.png)  

## Objectives  
Delete the user `carlos`  

## Background  
### `robots.txt`


## Steps  
### 1. 
I added `/robots.txt` into the URL and `Disallow` appeared    
![/Robots.txt](img/03-robots.png)

### 2. 
Now, I replaced `/robots.txt` with the value of `Disallow`: `/administrator-panel`  
I got administration, and I choose deleting user `Carlos`
![Users](img/04-users.png)  

And the lab was solved  
![Lab solved](img/05-lab-solved.png)
