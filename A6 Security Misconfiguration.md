# Security Misconfiguration

## Vulnerability

The application was running in DEVELOPMENT mode and due to lack of error handling, it sent the stack trace with internal file locations and other potentially sensitive information.


---
## Fix

Replace the `module.exports.calc` funcion 

```js
module.exports.calc = function (req, res) {
	//if (req.body.eqn) {
	//	res.render('app/calc', {
	//		output: mathjs.eval(req.body.eqn)
	//	})
	//} else {
		try{
			if (req.body.eqn) {
				res.render('app/calc', {
					output: mathjs.eval(req.body.eqn)
				})
			} else {
				res.render('app/calc', {
					output: 'Enter a valid math string like (3+3)*2'
				})
			}
		}catch(err){
		res.render('app/calc', {
			output: 'Enter a valid math string like (3+3)*2'
		})
	}
}
```
