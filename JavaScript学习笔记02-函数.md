#JavaScript学习笔记02-函数
声明
```
function functionName(parameters) {
	代码
}
```
例子

```
function myFunction(a, b) {
	return a * b;
}
```

函数表达式可以存储在变量中

```
var x = function (a,b) {return a * b};
```

在函数表达式存储在变量后，变量也可以作为一个函数使用

```
var x = function (a,b){return a * b};
var z = x(4, 3);
```
以上函数实际是一个匿名函数(函数没有名称)，存储在变量中，不需要函数名称，通常通过变量名来调用。

自调用函数
如果表达式后面紧跟(),则会自动调用。不能自调用声明的函数。通过添加括号，来说明它是一个函数表达式。

```
(function () {
	var x = "Hello!!";
})();
```
以上函数实际是一个匿名自我调用的函数(没有函数名)。

函数可以作为一个值使用

```
function myFunction(a, b){
	return a * b;
}
var x = myFunction(4, 3) * 2;
```

函数是对象
toString()方法将函数作为一个字符串返回。

#JavaScript函数参数
函数显式参数在函数定义时列出。
函数隐式参数在函数调用时传递给函数真正的值。

参数规则
JavaScript函数定义时显式参数没有指定数据类型。
JavaScript函数对隐式参数没有进行类型检测。
JavaScript函数对隐式参数的个数没有进行检测。

默认参数
如果函数在调用时未提供隐式参数，参数会默认设置为: undefined。
最好是设置一个默认值

```
function myFun(x, y) {
	if (y === undefined) {
		y = 0;
	}	
}
或者
function myFun(x, y) {
	y = y || 0;
}
```

##Arguments对象
JavaScript函数有个内置的对象arguments对象。argument对象包含了函数调用的参数数组。

```
x = findMax(1, 2, 4, 5, 6);
function findMax() {
	var i, max = arguments[0];
	
	if(arguments.length < 2) return max;
	
	for (i = 0; i < arguments.length; i++) {
		if (arguments[i] > max) {
			max = arguments[i];
		}
	}
	return max;
}
```

#JavaScript函数调用
JS函数有4种调用方式。每种方式的不同在于this的初始化。
在js中，this指向函数执行时的当前对象。
可以将函数定义为对象的方法。

```
var myObject = {
	firstName:"John",
	lastName:"Doe",
	fullName: function () {
		return this.firstName + " " + this.lastName;
	}
}
myObject.fullName();
```
函数作为对象方法调用，会使得this的值成为对象本身。

##使用构造函数调用函数
如果函数调用前使用了new关键字，则是调用了构造函数。看起来像是创建了新的函数，但实际上JavaScript函数是重新创建的对象。
```
//构造函数
function myFunction(arg1, arg2) {
	this.firstName = arg1;
	this.lastName = arg2;
}

var x = new myFunction("John", "Doe");
```

作为函数方法调用函数
在JavaScript中，函数是对象。JavaScript函数有它的属性和方法。
call()和apply()是预定义的函数方法。两个方法可用于调用函数，两个方法的第一个参数必须是对象本身。

```
function myFun(a,b){
	return a * b;
}
myObject = myFun.call(myObject, 10, 2);
```


