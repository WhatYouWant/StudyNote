#JavaScript HTML DOM(Documents Object Model 文档对象模型)
HTML DOM
当网页被加载时，浏览器会创建页面的文档对象模型（DOM）。
HTML DOM树
![](/Users/zhanyaali/GitClient/StudyNote/pic_htmltree.gif
)
通过可编程对象模型，js获得了足够的能力来创建动态HTML。
js可以改变页面中的所有html元素，属性，css样式。能够对页面中的所有事件做出反应。
查找HTML元素：通过id，标签名，类名都可以找到html元素。

```
var x=document.getElementById("intro"); //查找id=intro的元素
var y=x.getElementsByTagName("p"); //查找id=intro元素中所有<p>元素
var x=document.getElementsByClassName("intro");
```

在js中，document.write()可以用于直接向HTML输出流写内容。
改变HTML内容

```
document.getElmentBy(id).innerHTML=新的HTML
```

改变HTML属性

```
document.getElementById(id).attribute=新属性值
```

#改变CSS
改变HTML样式

```
document.getElementById(id).style.property=新样式
例
<script>
document.getElementById("p2").style.color="blue";
</script>
```

#JavaScript HTML DOM 事件
对事件作出反应
我们可以在事件发生时执行JavaScript，比如当点击某个html元素时

```
onclick=JavaScript
例如
<h1 onclick="this.innerHTML='Ooops!!'">点击文本</h1>
```

```
<script>
function changetext(id) {
	id.innerHTML="Ooops!";
}
</script>
<p onclick="changetext(this)">点击文本！<p>
```

#JavaScript HTML DOM EventListener
addEventListener()方法
addEventListener() 方法用于向指定元素添加事件句柄。
addEventListener() 方法添加的事件句柄不会覆盖已存在的事件句柄。
你可以向一个元素添加多个事件句柄。
你可以向同个元素添加多个同类型的事件句柄，如：两个 "click" 事件。
你可以向任何 DOM 对象添加事件监听，不仅仅是 HTML 元素。如： window 对象。
addEventListener() 方法可以更简单的控制事件（冒泡与捕获）。
当你使用 addEventListener() 方法时, JavaScript 从 HTML 标记中分离开来，可读性更强， 在没有控制HTML标记时也可以添加事件监听。
你可以使用 removeEventListener() 方法来移除事件的监听。

```
element.addEventlistener(event, function, useCapture);
```
第一个参数是事件的类型 (如 "click" 或 "mousedown").
第二个参数是事件触发后调用的函数。
第三个参数是个布尔值用于描述事件是冒泡还是捕获。该参数是可选的。

事件冒泡或事件捕获
事件传递有两种方式：冒泡与捕获。
事件传递定义了元素事件触发的顺序。 如果你将 <p> 元素插入到 <div> 元素中，用户点击 <p> 元素, 哪个元素的 "click" 事件先被触发呢？
在 冒泡 中，内部元素的事件会先被触发，然后再触发外部元素，即： <p> 元素的点击事件先触发，然后会触发 <div> 元素的点击事件。
在 捕获 中，外部元素的事件会先被触发，然后才会触发内部元素的事件，即： <div> 元素的点击事件先触发 ，然后再触发 <p> 元素的点击事件。
默认值为 false, 即冒泡传递，当值为 true 时, 事件使用捕获传递。

removeEventListener()方法移除由addEventListener()方法添加的事件句柄。

#JavaScript HTML DOM元素（节点）
创建新的HTML元素。

```
<body>
<div id="div1">
<p id="p1">这是一个段落。</p>
<p id="p2">这是另一个段落。</p>
</div>
<script>
var para=document.createElement("p"); //创建新p元素
var node=document.createTextNode("这是一个新段落。"); //创建文本节点，向p元素添加文本。
para.appendChild(node); //向p元素追加文本节点
var element=document.getElementById("div1"); //找到一个已有元素
element.appendChild(para); //在已存在的元素后添加新元素。
</script>
</body>
```


