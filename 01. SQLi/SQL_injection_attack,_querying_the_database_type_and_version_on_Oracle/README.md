# Write-up: SQL injection attack, querying the database type and version on Oracle  
![Homepage](img/01-homepage.png)  
Lab-link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle  
Difficulty: Practitioner  
Status: Solved  

## Lab description  
![Lab description](img/02-lab-description.png)  

## Objectives  
Display Oracle version string using UNION attack  

## Background  
On **Oracle** databases, every `SELECT` statement must specify a table to select `FROM`. If your `UNION SELECT` attack does not query from a table, you will still need to include the `FROM` keyword followed by a valid table name.
There is a built-in table on **Oracle** called `dual` which you can use for this purpose.
**Example query:** 
```sql
UNION SELECT 'abc' FROM dual
```
