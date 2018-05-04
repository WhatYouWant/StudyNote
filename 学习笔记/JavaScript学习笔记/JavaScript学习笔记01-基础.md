#JavaScript学习笔记01-基础
JavaScript对大小写敏感。 JavaScript会忽略多余的空格。可以使用反斜杠对代码进行折行。
##在<head>或者<body>的JavaScript
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
使用关键字var来定义变量。
##JavaScript语句
语句用分号隔开。
#JavaScript注释
单行注释: //； 多行注释: /* xxx */






