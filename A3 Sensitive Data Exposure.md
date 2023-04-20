# Sensitive Data Exposure 

### Vulnerability
~~~
The Admin API endpoint at `app/admin/api/users` sends the entire user object to the front end.
~~~

---

### Fix

### Hashed Passwords Disclosed
#### Edits to the `core/appHandler.js`


```js
// Remove the code that sends entire user object
db.User.findAll({}).then(users => {

// Add following to only get required non-sensitive data
db.User.findAll({attributes: [ 'id' ,'name', 'email']}).then(users => {
		
```

---

### Logging of sensitive information
#### Edits to the `models/index.js`

```js
// Add a semi semicolon to the end of following line 
dialect: config.dialect,
// Add the new line to make logging false
logging: false
```
