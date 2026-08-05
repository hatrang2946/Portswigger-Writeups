# Write-up: SQL injection attack, querying the database type and version on Oracle  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle  
Difficulty: Practitioner  
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png)  

## Objectives  
Retrieve the Oracle database version using UNION-based SQL injection  

## Background  
### UNION-based SQL injection  
`UNION` combines the results of two `SELECT` query into a single result set  
The queries before and after `UNION` must have the same number of columns and same data types

### Finding the number of columns  
A common technique is using  
```text
ORDER BY 1
ORDER BY 2
ORDER BY 3
...
ORDER BY n
```  
to know the number of columns 

### Oracle-specific note
On **Oracle** databases, every `SELECT` statement must specify a table to select `FROM`. If your `UNION SELECT` attack does not query from a table, you will still need to include the `FROM` keyword followed by a valid table name.  
There is a built-in table on **Oracle** called `dual` which you can use for this purpose.
**Example query:** 
```sql
UNION SELECT 'abc' FROM dual
```

## Steps  
Firstly, I choose Gifts for category 
In Burp Suite, I send GET request to Repeater
I add 
```text
' UNION ORDER BY 1--
```
then using 2, 3 to know the number of columns of the table

Because this lab uses Oracle, it needs `FROM`. Then I inserted dual
