# Write-up: SQL injection attack, querying the database type and version on MySQL and Microsoft  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft  
Difficulty: Practitioner  
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png)  

## Objectives  
Retrieve the MySQL and Microsoft database version using UNION-based SQL injection

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
to determine the number of columns  

### MySQL and Microsoft specific note
It is better to use `#` to comment instead of `--` in this lab

## Steps  
