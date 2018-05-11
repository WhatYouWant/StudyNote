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
#JavaScript运算符
用于字符串的+运算符
+运算符用于把文本值或字符串变量加起来（连接起来）。
对字符串和数字进行加法运算，结果将成为字符串。
#JavaScript 比较和逻辑运算符
`===` 绝对等于（值和类型均相等）

#JavaScript的 typeof, null, undefined
typeof用来检测变量的数据类型。

```
typeof "John" //返回string
typeof 3.14 	//返回number
```
null,表示什么都没有。
null是一个只有一个值的特殊类型，表示一个空对象引用。
可以设置对象为null来清空对象。可以设置为undefined来清空对象。
```
var person = null; //值为null(空), 但类型为对象。
var person = undefined; //值为undefined, 类型为unfefined
```
undefined是一个没有设置值的变量。typeof一个没有值的变量会返回undefined。
undefined和null区别：值相等，但是类型不等。

#JavaScript类型转换
5种数据类型：string, number, boolean, object, function
3种对象类型: Object, Date, Array
2个不包含任何值的数据类型: null, undefined

instanceof
 
```
var arr = [1,2,3];
if (arr instanceof Array) {
	..
}
```
constructor属性，返回所有JavaScript变量的构造函数。

```
“John”.constructor //返回函数 String() {[native code]}
```
数字转字符串，String()可以将数字转为字符串。Number方法toString()也有同样的效果。

#JavaScript正则表达式
正则表达式使用单个字符来描述，匹配一系列符合某个句法规则的字符串搜索模式。
搜索模式可用于文本搜索和文本替换。
##语法
```
/正则表达式主体/修饰符(可选)
```
例

```
var patt = /runoob/i
```
`runoob/i`是一个正则表达式。
runoob 是一个正则表达式主体(用于检索)。
i 是一个修饰符（搜索不区分大小写）。

使用字符串方法。
search()方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回子串的起始位置。
replace()方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。
例如

```
var str = "Visit Runoob!";
var n = str.search(/Runoob/i); //搜索主体为正则表达式
var n = str.search("Runoob"); //搜索主题为字符串
```
##正则表达式修饰符

修饰符 | 描述
------- | -------
i | 执行对大小写不敏感的匹配。
g | 执行全局匹配(查找所有匹配而非找到第一个匹配后就停止)。
m | 执行多行匹配。

##正则表达式模式
方括号用于查找某个范围的字符。

表达式 | 描述
------- | -------
[abc] | 查找方括号之间的任意字符
[0-9] | 查找任何从0到9的数字
(x|y) | 查找任何以 | 分割的选项。

元字符是拥有特殊含义的字符：

元字符 | 描述
------- | -------
\d | 查找数字。
\s | 查找空白字符。
\b | 匹配单词边界。
\uxxxx | 查找以十六进制数xxxx规定的Unicode字符。

量词：

量词 | 描述
------- | -------
n+ | 匹配任何包含至少一个n的字符串。
n* | 匹配任何包含零个或多个n的字符串。
n? | 匹配任何包含零个或一个n的字符串。

##RegExp对象
在JS中，RegExp对象是一个预定义了属性和方法的正则表达式对象。
##使用test()
test() 方法是一个正则表达式方法。
test() 方法用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回 true，否则返回 false。
```
var patt1=new RegExp("e");
var result = patt1.test("The best things in life are free");
```
##使用exec()
exec() 方法是一个正则表达式方法。
exec() 方法用于检索字符串中的正则表达式的匹配。
该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。
正则表达式表单验证实例：

```
//是否带小数
function isDecimal(strValue) {
	var objRegExp = /^\d+\.\d+$/;
	return objRegExp.test(strValue);
}

//校验是否中文名称组成
function ischian(str) {
	var reg=/^[\u4E00-\u9FA5]{2,4}$/;
	return reg.test(str);
}

//校验是否全由8位数字组成
function isStudentNo(str) {
	var reg=/^[0-9]{8}$/;
	return reg.test(str);
}

//校验电话码格式
function isTelCode(str) {
	var reg = \^((0\d{2,3}-\d{7,8})|(1[3584]\d{9}))$/;
	return reg.test(str);
}

//校验邮件地址是否合法
function IsEmail(str) {
	var reg=/^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(\.[a-zA-Z0-9_-])+$/;
	return reg.test(str);
}
```

#JavaScript错误
try 语句允许我们定义在执行时进行错误测试的代码块。
catch 语句允许我们定义当 try 代码块发生错误时，所执行的代码块。
JavaScript 语句 try 和 catch 是成对出现的。
throw 语句允许我们创建自定义错误。
语法

```
try {
	//...运行代码 throw ...
} catch(err) {
	//...处理错误
}
```
例子

```
function myFunction() {
    var message, x;
    message = document.getElementById("message");
    message.innerHTML = "";
    x = document.getElementById("demo").value;
    try { 
        if(x == "")  throw "值为空";
        if(isNaN(x)) throw "不是数字";
        x = Number(x);
        if(x < 5)    throw "太小";
        if(x > 10)   throw "太大";
    }
    catch(err) {
        message.innerHTML = "错误: " + err;
    }
}
```

#JavaScript变量提升
JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。
JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。

JavaScript 只有声明的变量会提升，初始化的不会。











