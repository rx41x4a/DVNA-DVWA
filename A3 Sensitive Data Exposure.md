# Fix

## 
`core/appHandler.js`


```
// Remove the code that sends entire user object
db.User.findAll({}).then(users => {

// Add following to only get required non-sensitive data
db.User.findAll({attributes: [ 'id' ,'name', 'email']}).then(users => {
		
```
