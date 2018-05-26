#JS 浏览器BOM
浏览器对象模型(BOM)使JavaScript有能力与浏览器“对话”。
##JavaScript Window
Window对象
所有浏览器都支持window对象。它表示浏览器窗口。所有js全局对象，函数以及变量均自动成为window对象的成员。全局变量时window对象的属性，全局函数是window对象的方法。

获取浏览器窗口的尺寸

```
var w=window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth
var h=window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
```
##JavaScript Window Screen
window.screen对象包含有关用户屏幕的信息。
可以不使用window这个前缀。
screen.availWidth:可用的屏幕宽度。
screen.availHeight:可用的屏幕高度。

##JavaScript Window Location
window.location对象用于获得当前页面的地址（URL），并把浏览器重定向到新的页面。
可以不使用前缀
location.hostname:web主机的域名。
location.pathname:当前页面的路径和文件名。
location.port:web主机的端口。
location.protocol:使用的web协议。（http或https）
location.href:返回当前页面的url。
location.assign()方法加载新的文档。
##JavaScript Window History
window.history对象包含浏览器的历史。
可以不使用window前缀。
history.back():后退
history.forward():前进
##JavaScript Navigator
window.navigator包含有关访问者浏览器的信息。
##JavaScript 弹窗
三种消息框：警告框，确认框，提示框。
警告框,可以直接使用alert
window.alert("sometext");

确认框
window.confirm("sometext");

```
var r=confirm("按下按钮");
if(r==true){
	x="你按下了\"确定\"按钮";
} else {
	x="取消";
}
```

提示框
提示框用于提示用户在进入页面前输入某个值。
window.prompt("sometext", "defaultvalue");

```
var person=prompt("请输入你的名字","Harry Potter");
if (person!=null && person != "") {
	x="你好" + person + "!今天感觉如何";
	document.getElementById("demo").innerHTML=x;
}
```
换行： \n
##JavaScript 计时事件

```
setInterval()-间隔指定的毫秒数不停地执行指定的代码。
setTimeout()-在指定的毫秒数后执行指定代码。
```
setInterval()方法:

```
window.setInterval("javascript function", milliseconds);
```
例子：

```
var myVar=setInterval(function(){myTimer()},1000);
function myTimer(){
	var d=new Date();
	var t=d.toLocaleTimeString();
	document.getElementById("demo").innerHTML=t;
}
```
停止执行：clearInterval();
语法

```
window.clearInterval(intervalVariable);
```

setTimeout():

```
myVar=window.setTimeout("javascript function", milliseconds);
```
停止执行

```
window.clearTimeout(timeoutVariable);
```
##JavaScript Cookie
cookie用于存储web页面的用户信息。
cookie以键值对形式存在
创建一个函数来存储访问者的名字

```
function setCookie(cname, cvalue, exdays) {
	var d = new Date();
	d.setTime(d.getTime()+(exdays*24*60*60*1000));
	var expires = "expires="+d.toGMTString();
	document.cookie = cname + "=" + cvalue + ";" + expires;
}
```

获取cookie值的函数

```
function getCookie(cname) {
	var name = cname + "=";
	var ca = document.cookie.split(';');
	for(var i = 0; i < ca.length; i++) {
		var c = ca[i].trim();
		if(c.indexOf(name)==0) 
			return c.substring(name.length,c.length);
	}
	return "";
}
```

检测cookie值的函数

```
function checkCookie() {
	var username=getCookie("username");
	if(username!=""){
		alert("Welcome again" + username);
	} else {
		username = prompt("Please enter your name:","");
		if(username!="" && username!=null) {
			setCookie("username", username, 365);
		}
	}
}
```

