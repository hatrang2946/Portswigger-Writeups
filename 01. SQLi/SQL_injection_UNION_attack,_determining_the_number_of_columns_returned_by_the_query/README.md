# Write-up: SQL injection UNION attack, determining the number of columns returned by the query   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns  
Difficulty: Practitioner  
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives  
Retrieve the number of columns returned by the query using UNION-based SQL injection

## Background  
### UNION-based SQL injection  
`UNION` combines the results of two `SELECT` queries into a single result set  
The queries before and after `UNION` must have the same number of columns and compatible data types

### Finding the number of columns  
A common technique is using  
```text
ORDER BY 1
ORDER BY 2
ORDER BY 3
...
ORDER BY n
```  
to determine the number of columns that are being returned by the query  

## Steps
### 1. Establish a baseline  
Firstly, I chose the Pets category  
In Burp Suite, I intercepted this request and sent it to Repeater  
![Request](img/03-request.png)  

### 2. Determine the number of columns
By injecting the following payloads one by one: 
```text
' ORDER BY 1--
...
```
I got that there were 3 columns  

### 3. Attack with UNION-based SQLi
Since there were 3 columns, the `UNION SELECT` statement must also return 3 columns.  
I injected:  
```text
'+UNION+SELECT+NULL,NULL,NULL--
```
And the lab was solved   
![Lab solved](img/04-lab-solved.png)
