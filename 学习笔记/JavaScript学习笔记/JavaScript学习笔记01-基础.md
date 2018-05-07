#JavaScript学习笔记01-基础
JavaScript对大小写敏感。 JavaScript会忽略多余的空格。可以使用反斜杠对代码进行折行。
##在 head 或者 body 中的JavaScript
HTML 中的脚本必须位于 <script> 与 </script> 标签之间。
脚本可被放置在 HTML 页面的 <body> 和 <head> 部分中。
通常的做法是把函数放入 <head> 部分中，或者放在页面底部。这样就可以把它们安置到同一处位置，不会干扰页面的内容。
##外部的JavaScript
也可以把脚本保存到外部文件中。外部文件通常包含被多个网页使用的代码。
外部 JavaScript 文件的文件扩展名是 .js。
如需使用外部文件，请在 script 标签的 "src" 属性中设置该 .js 文件。

```
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
	
<h1>我的 Web 页面</h1>
<p id="demo">一个段落。</p>
<button type="button" onclick="myFunction()">点击这里</button>
<p><b>注释：</b>myFunction 保存在名为 "myScript.js" 的外部文件中。</p>
<script src="myScript.js"></script>
	
</body>
</html>
```

myScript.js文件代码如下:
```
function myFunction()
{
    document.getElementById("demo").innerHTML="我的第一个 JavaScript 函数";
}
```

#JavaScript输出
JS显示数据，可以通过以下几种方式

```
* 使用window.alert()弹出警告框
* 使用document.write()方法将内容写到HTML文档中。
* 使用innerHTML写入到HTML元素。
* 使用console.log()写入到浏览器的控制台。
```
附：请使用 document.write() 仅仅向文档输出写内容。如果在文档已完成加载后执行document.write，整个 HTML 页面将被覆盖。

#JavaScript语法
##JavaScript字面量（常量）
一般固定值成为字面量。
```
* 数字（Number）字面量：整数或者小数。
* 字符串（string）字面量：单引号或者双引号。
* 表达式字面量：用于计算
* 数组（array）字面量：定义一个数组。[1,2,3,4,5]
* 对象（Object）字面量：定义一个对象。{firstName:"John", lastName:"Doe"}
* 函数（Function）字面量：定义一个函数。 function myFunction(a, b) {return a*b;}
```

##JavaScript变量
使用关键字var来定义变量。对大小写敏感。
##JavaScript语句
语句用分号隔开。
#JavaScript注释
单行注释: //； 多行注释: /* xxx */

#JavaScript变量
##let变量
let允许你声明一个作用域被限制在块级中的变量、语句或者表达式。在Function中局部变量推荐使用let变量，避免变量名冲突。
作用域规则：let 声明的变量只在其声明的块或子块中可用，这一点，与var相似。二者之间最主要的区别在于var声明的变量的作用域是整个封闭函数。

```
function varTest() {
    var x = 1;
    if (true) {
        var x = 2;       // 同样的变量!
        console.log(x);  // 2
    }
    console.log(x);  // 2
}

function letTest() {
    let x = 1;
    if (true) {
        let x = 2;       // 不同的变量    
        console.log(x);  // 2  
    }
    console.log(x);  // 1
}
```

#JavaScript数据类型
字符串（String）、数字(Number)、布尔(Boolean)、数组(Array)、对象(Object)、空（Null）、未定义（Undefined）。

js拥有动态类型，意味着相同的变量可以作用于不同的类型。

```
var x;
var x = 5;
var x = "John";
```
字符串，存储字符，使用单引号或者双引号。 var answer = "Hello";
数字，可以带小数点也可以不使用小数点。
布尔，只能有两个值，true或false。
数组

```
var cars = new Array();
cars[0]="Saab";
或者（condensed array）
var cars = new Array("Saab", "Volvo");
或者(literal array)
var cars = ["Saab", "Volvo"];
```
对象：由花括号分隔，在括号内部，以名称和值对的形式（name:value）来定义。属性由逗号分隔：

```
var person={firstname:"John", lastname:"Doe", id:5566};
```
空格和折行无关紧要

```
var person={
firstname : "John",
lastname : "Doe",
id : 5566
};
```
寻址方式：

```
name=person.lastname;
name=person["lastname"];
```
Undefined和Null
Undefined表示变量不含有值，可以通过将变量的值设置为null来清空变量。
声明变量类型，声明新变量时，可以使用new来声明其类型。
var carname = new String;

#JavaScript对象
js中几乎所有事物都是对象。
定义

```
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
```
对象属性
“JavaScript对象是变量的容器。”
JavaScript对象是键值对的容器。

对象方法
对象方法通过添加()调用。
该实例访问了person对象的fullName（）方法：
```
name = person.fullName();
```
如果要访问person对象的fullName属性，将作为一个定义函数的字符串返回：
```
name=person.fullName;
```

创建对象方法:
methodName : function() { code lines }
访问对象方法：
objectName.methodName();

实例

```
定义含有属性，方法的对象
...
<script>
var person = {
	firstName : "John",
	lastName : "Doe",
	id : 5566,
	fullName : function()
	{
		return this.firstName + " " + this.lastName;
	}
}
</script>
...
使用对象
document.getElementById("demo").innerHTML = person.fullName();
```

#JavaScript函数
函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。
函数语法，包含在花括号中的代码块，前面使用关键词function：

```
function functionname() {
	//执行代码
}
```

调用带参数的函数
声明函数时，把参数作为变量来声明：
function myFunction(var1, var2) {
...代码
}
```
<button onclick="myFunction('Harry Potter', 'Wizard')">点击这里</button>
<script>
function myFunction(name, job) {
	alert("welcome" + name + ", the" + job);
}
</script>
```

带有返回值的函数

```
function myFounction() {
	var x = 5;
	return x;
}
```

函数内未声明即使用的变量

```
function func(){
	undefined_var=110;
}
```
在 func() 被第一次调用之前， undefined_var 变量是不存在的即 undefined。func() 被调用过之后，undefined_var 成为全局变量。
#JavaScript事件
HTML 事件是发生在 HTML 元素上的事情。
当在 HTML 页面中使用 JavaScript 时， JavaScript 可以触发这些事件。

HTML元素中可以添加事件属性，使用JavaScript代码来添加HTML元素。
单引号
```
<some-HTML-element some-event='JavaScript 代码'>
```
双引号

```
<some-HTML-element some-event="JavaScript代码">
```

#JavaScript字符串
用于存储和处理文本。可以使用单引号或者双引号。
可以使用索引来访问字符串中的每个字符。

```
var carname = "Volvo xc60";
var character = carname[5];
var sln = carname.length;
```









