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
robots.txt is a file that provides instructions to search engine crawlers about which URLs they should avoid crawling. However, it does not restrict access to those URLs.

## Steps  
### 1. Discover the admin panel path  
I added `/robots.txt` into the lab URL and found a `Disallow` directive that revealed the admin panel path     
![/Robots.txt](img/03-robots.png)

### 2. Access the admin panel  
Now, I replaced `/robots.txt` with `/administrator-panel` to access the admin panel.  
![Users](img/04-users.png)  
The panel was accessible without requiring administrator authentication.  

### 3. Delete `Carlos` user  
I clicked `Delete` option next to the user `Carlos`.

And the lab was solved  
![Lab solved](img/05-lab-solved.png)
