#JavaScript对象
JavaScript是面向对象的语言，但JavaScript不使用类。
在JavaScript中，不会创建类，也不会通过类来创建对象。
JavaScript基于prototype，而不是基于类的。

##JavaScript for...in 循环
for...in语句循环遍历对象的属性。
例子：
```
function myFunction(){
	var x;
	var txt="";
	var person={fname:"Bill", lname:"Gates",age:56};
	for(x in person){
		txt=txt + person[x];
	}
	document.getElementById("demo").innerHTML=txt;
}
```

## Number对象
所有数字均为64位，不分类型。
精度：整数最多为15位，小数最大为17位。

可以使用toString()方法输出16进制，8进制，2进制。

```
var myNumber=128;
myNumber.toString(16);
myNumber.toString(8);
myNumber.toString(2);
```

无穷大：Infinity
负无穷大: -Infinity

非数字值： NaN
NaN 属性是代表非数字值的特殊值。该属性用于指示某个值不是数字。可以把 Number 对象设置为该值，来指示其不是数字值。
你可以使用 isNaN() 全局函数来判断一个值是否是 NaN 值。

##数字可以是数字或者对象
数字可以私有数据进行初始化，就像x=123；
JavaScript数字对象初始化数据， var y = new Number(123);

##JavaScript字符串对象(String)
##JavaScript Boolean对象
如果布尔对象无初始值或者其值为: 0, -0, null, "", false, undefined, Nan, 则为false，否则为true。
##JavaScript Math（算数）对象
Math对象的作用是，执行常见的算数任务。
8种可被Math对象访问的算数值： Math.E, Math.PI, Math.SQRT2, Math.SQRT1_2, Math.LN2, Math.LN10, Math.LOG2E, Math.LOG10E

#JavaScript RegExp对象（正则表达式 Regular Expression）
语法

```
var patt = new RegExp(pattern, modifiers);
或者
var patt=/pattern/modifiers;
```
模式描述了一个表达式模型。
修饰符(modifiers)描述了检索是否是全局，区分大小写等。

```
var re = new RegExp("\\w+");
var re = /\w+/;
```

RegExp修饰符
修饰符用于执行不区分大小写和全文的搜索。
i - 修饰符是用来执行不区分大小写的匹配。
g - 修饰符是用于执行全文的搜索（而不是在找到第一个就停止查找,而是找到所有的匹配）。

例子

```
var str = "Visit RUnoob";
var patt1 = /runoob/i;
document.write(str.match(patt1));
```
运行结果： RUnoob


```
var str="Is this all there is?"
var patt1=/is/gi;
```

test()方法搜索字符串指定的值，根据结果并返回真或假。

```
var patt1 = new RegExp("e");
document.write(patt1.test("The best things in life are free"));
```


