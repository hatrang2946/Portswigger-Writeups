# Write-up: Visible error-based SQL injection   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based  
Difficulty: Practitioner   
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives 
Retrieve the password of `administrator` user and log into their account  

## Background  
### Blind SQL injection   
Blind SQL injection occurs when an application is vulnerable to SQL injection, but the results of the injected query are not directly displayed in the response. Instead, information must be inferred from differences in the application's behavior.   
In this lab, an SQL error is intentionally triggered when error messages appeared in the response. These error messages would reveal the information of the `administrator` account.

### `CAST`   

### `SELECT` subqueries  
A subquery is a SQL query nested inside another query.  
In this lab, subqueries are used to check information from the `users` table:  
```sql
SELECT 'x' FROM users WHERE username='administrator'
```
The result can then be used in a Boolean condition:  
```sql
(SELECT 'x' FROM users WHERE username='administrator')='x'
```

### Why use LIMIT 1?  
`LIMIT 1` restricts the subquery to a single row:  
```sql
SELECT 'x' FROM users LIMIT 1
```
For this attack, we only need to know whether at least one row exists, so returning multiple rows is unnecessary.

## Steps   
### 1. Establish a baseline  
Firstly, I chose the **Pets** category
In Burp Suite, I intercepted this request and sent it to Repeater  
![Request](img/03-request.png)   
The important parameter in this lab is the `trackingId` cookie    

### 2. Confirm the parameter is vulnerable to blind SQL injection   
I injected `'` index after the value of trackingId    
For example: `TrackingId=jg7XrsaVWonmuCh0'`  
In the response, I saw the message containing the whole query and the value of trackingId. 

### 3. 
