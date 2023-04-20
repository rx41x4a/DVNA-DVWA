# Sensitive Data Exposure 

## Vulnerability

###### Hashed Passwords Disclosed. The Admin API endpoint at `app/admin/api/users` sends the entire user object to the front end including password hash.


---

## Fix

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
