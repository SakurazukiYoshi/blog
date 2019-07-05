---
title: js的基础
date: 2017-05-02 18:17:01
categories: js
---

# script

## script的加载

	<script type="text/javascript" src="afile.js">
		alert("hello world");
	</script>

这样写只会加载外部引入的代码，中间的代码并不会执行

## script存放位置


通常放在head中，但这样会：

1. 全部 JavaScript 代码都被下载
2. 解析和执行完成
3. 呈现页面的内容

结果会导致：浏览器在呈现页面时出现明显的延迟，此时浏览器窗口中将是一片空白。


实际上都放在`</body>`标签的前面，用户也会因为浏
览器窗口显示空白页面的时间缩短而感到打开页面的速度加快了


<!--more--><!--more-->


## script属性  

### defer

只是试用于外部脚本文件，html5忽略了这个属性。

表明脚本在执行时不会影响页面的构造，脚本会被延迟到整个页面都解析完毕后再运行。带有defer属性的script浏览器立即下载，但延迟执行。


缺点：延迟脚本并不一定会按照顺序执行，所以最好只包含一个defer属性


###  async 

只适用于外部脚本文件，和defer类似，区别 async 的脚本并不保证按照指定它们的先后顺序执行。相互依赖的js不能使用

## 嵌入外部文件的好处

1. 可维护， JavaScript 文件都放在一个文件夹中，没有html代码
2. 可缓存：加快页面加载的速度
3. 无须区分XHTML和HTML的区别来写


# 文档模式
文档类型（doctype）切换实现，主要影响 CSS内容的呈现；文档开始处没有发现文档类型声明，则所有浏览器都会默认开启混杂模式，这样不推荐，因为不同浏览器在这种模式下的行为差异非常大。

## 混杂模式（quirks mode）
混杂模式会让 IE 的行为与（包含非标准特性的）IE5 相同


## 标准模式（standards mode）
标准模式则让 IE 的行为更接近标准行为

## 准标准模式（almost standards mode）

不标准的地方主要体现在处理图片间隙的时候（在表格中使用图片时问题最明显）


# 关键字和保留字

## 关键字


	break do instanceof typeof
	case else new var
	catch finally return void
	continue for switch while
	debugger* function this with
	default if throw
	delete in try


## 保留字


	abstract enum int short
	boolean export interface static
	byte extends long super
	char final native synchronized
	class float package throws
	const goto private transient
	debugger implements protected volatile
	double import public

# 变量

## 定义多个变量


	var message = "hi"
	found = false,
	age = 29;

## 类型


	"undefined"——如果这个值未定义，有值但是未定义
	"Null"——空对象指针，定义的用于保存对象变量
	"boolean"——如果这个值是布尔值；
	"string"——如果这个值是字符串；
	"number"——如果这个值是数值；
	"object"——如果这个值是对象或 null；
	"function"——如果这个值是函数。

## 检查一个变量是否已经保存了一个对象

	if (car != null){
		// 对 car 对象执行某些操作
	}

## 布尔值


	Boolean 		true 			false
	String 		任何非空字符串 		""（空字符串）
	Number   	任何非零数（无穷大）   0和NaN
	Object 		任何对象 			null
	Undefined 	n/a① 				undefined

## 进制

	八进制： 0
	十六进制：0x

## 小数

保存浮点数值需要的内存空间是保存整数值的两倍，如果小数点后面没有跟任何数字，那么这个数值就可以作为整数值来保存。

	var floatNum1 = 1.; // 小数点后面没有数字——解析为 1
	var floatNum2 = 10.0; // 整数——解析为 10


## 指数

e表示10的多少次方并不是2.7多少

	var floatNum = 3.125e7; // 等于 31250000


浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。例如， 0.1 加 0.2的结果不是 0.3，而是0.30000000000000004

	if (a + b == 0.3){ // 不要做这样的测试！
		alert("You got 0.3.");
	}

## 数值的范围

	最小：5e-324
	最大：1.7976931348623157e+308

	Infinity无穷大

## NaN

Not a Number，本来要返回数值的操作数未返回数值的情况


NaN 与任何值都不相等，包括 NaN 本身


判断：

	alert(isNaN(NaN));

##  数值转换


-`Number() `

	可以用于所有数据类型
	
	如果是 Boolean 值， true 和 false 将分别被转换为 1 和 0。
	如果是数字值，只是简单的传入和返回。
	如果是 null 值，返回 0。
	如果是 undefined，返回 NaN。
	如果是字符串，遵循下列规则：
		如果字符串中只包含数字（包括前面带正号或负号的情况），则将其转换为十进制数值，即"1"会变成 1， "123"会变成 123，而"011"会变成 11（注意：前导的零被忽略了）；

		如果字符串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（同样，也会忽略前导零）；

		如果字符串中包含有效的十六进制格式，例如"0xf"，则将其转换为相同大小的十进制整数值；

		如果字符串是空的（不包含任何字符），则将其转换为 0；

		如果字符串中包含除上述格式之外的字符，则将其转换为 NaN。


eg：
	var num1 = Number("Hello world!"); //NaN
	var num2 = Number(""); //0
	var num3 = Number("000011"); //11
	var num4 = Number(true); //1


-`parseInt(a，b)`

a表示数据，b表示进制。

常用，只是用于字符串类型转换为数字

	var num1 = parseInt("1234blue"); // 1234
	var num2 = parseInt(""); // NaN
	var num3 = parseInt("0xA"); // 10（十六进制数）
	var num4 = parseInt(22.5); // 22
	var num5 = parseInt("070"); // 56（八进制数）
	var num6 = parseInt("70"); // 70（十进制数）
	var num7 = parseInt("0xf"); // 15（十六进制数）


	var num1 = parseInt("10", 2); //2 （按二进制解析）
	var num2 = parseInt("10", 8); //8 （按八进制解析）
	var num3 = parseInt("10", 10); //10 （按十进制解析）
	var num4 = parseInt("10", 16); //16 （按十六进制解析）

-`parseFloat()`

转为小数，但是注意字符串中的第一个小数点是有效的，而第二个小数点就是无效的了

	var num1 = parseFloat("1234blue"); //1234 （整数）
	var num2 = parseFloat("0xA"); //0
	var num3 = parseFloat("22.5"); //22.5
	var num4 = parseFloat("22.34.5"); //22.34
	var num5 = parseFloat("0908.5"); //908.5
	var num6 = parseFloat("3.125e7"); //31250000

## String


双引号和单引号不会影响对字符串，但是以双引号开头的字符串也必须以双引号结尾，而以单引号开头的字符串必须以单引号结尾。

	var firstName = 'Nicholas"; // 语法错误（左右引号必须匹配）


字符串是不可变的，改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量。


###	转换为字符串

-变量.toString（） 可以传一个参数表示进制

	
	var num = 10;
	alert(num.toString()); // "10"
	alert(num.toString(2)); // "1010"
	alert(num.toString(8)); // "12"
	alert(num.toString(10)); // "10"
	alert(num.toString(16)); // "a"

## 一元加和减操作符


	num = +num;   //对数字于不变


	//对于字符串转化为数值

	var s1 = "01";
	var s2 = "1.1";
	var s3 = "z";
	var b = false;
	var f = 1.1;
	var o = {
	valueOf: function() {
	return -1;
	}
	};
	s1 = +s1; // 值变成数值 1
	s2 = +s2; // 值变成数值 1.1
	s3 = +s3; // 值变成 NaN
	b = +b; // 值变成数值 0
	f = +f; // 值未变，仍然是 1.1
	o = +o; // 值变成数值-1	


## 二元

	(3>2)&&alert(1);	//  a&&b  如果a为真就执行b 相当于if（a）{b}

	a&&b   a真返回b，a假返回a


同理 

	a||b相当于  if(a)else{b}

## 字符串的比较


是比较的两个首字母的字符编码

	var result = "23" < "3"; //true

	"2"的字符编码是 50，而"3"的字符编码是 51

注意一边是字符串一边是数字的情况，字符串会被转化为数字

	"2"的字符编码是 50，而"3"的字符编码是 51



任何操作数与 NaN 进行关系比较，结果都是 false


	null == undefined true
 	null === undefined  false
	"NaN" == NaN false
	5 == NaN false
	NaN == NaN false
	NaN != NaN true
	false == 0 true
	true == 1 true
	true == 2 false
	undefined == 0 false
	null == 0 false
	"5"==5 true

## 循环

	for (;;) { // 无限循环
	doSomething();
	}

## switch

	switch (expression) {
		case value: statement
		break;
		case value: statement
		break;
		case value: statement
		break;
		case value: statement
		break;
		default: statement
	}

如果表达式等于这个值（value），则执行后面的语句（statement）


用switch为了避免这种情况：


	if (i == 25){
		alert("25");
	} else if (i == 35) {
		alert("35");
	} else if (i == 45) {
		alert("45");
	} else {
		alert("Other");
	}

	switch (i) {
		case 25:
		alert("25");
		break;
		case 35:
		alert("35");
		break;
		case 45:
		alert("45");
		break;
		default:
		alert("Other");
	}
				
	switch (i) {
		case 25:
		/* 合并两种情形 */
		case 35:
		alert("25 or 35");
		break;
		case 45:
		alert("45");
		break;
		default:
		alert("Other");
	}

判断多少的写法：


	var num = 25;
		switch (true) {
		case num < 0:
		alert("Less than 0.");
		break;
		case num >= 0 && num <= 10:
		alert("Between 0 and 10.");
		break;
		case num > 10 && num <= 20:
		alert("Between 10 and 20.")
		break;
		default:
		alert("More than 20.");
	}


switch 语句在比较值时使用的是全等操作符，因此不会发生类型转换


## return


推荐的做法是要么让函数始终都返回一个值，要么永远都不要返回值。否则，如果函数有时候返回值，有时候有不返回值，会给调试代码带来不便。

	function sayHi(name, message) {
		return;
		alert("Hello " + name + "," + message); //永远不会调用
	}

## arguments的妙用

	function sayHi() {
		alert("Hello " + arguments[0] + "," + arguments[1]);
	}

实现参数的隐藏使用


	function doAdd() {
		if(arguments.length == 1) {
		alert(arguments[0] + 10);
		} else if (arguments.length == 2) {
		alert(arguments[0] + arguments[1]);
	}
	}
	doAdd(10); //20
	doAdd(30, 20); //50

利用不同的参数来实现不同的功能


# 作用域

	变量=基本类型+引用类型

	内存=堆（存放引用类型）+栈（存放简单的数据+指针）

栈的优势就是存取速度比堆要快，仅次于直接位于CPU中的寄存器，但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。

堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，垃圾收集器会自动地收走这些不再使用的数据，但是缺点是由于在运行时动态分配内存，所以存取速度较慢。

## 基本类型

	undefined、numll、boolean、number、string
	直接改变内存的值

![](http://www.2cto.com/uploadfile/Collfiles/20150623/20150623100326164.png)

	每一个的值是相互独立的
	
	var num1 = 5;
	var num2 = num1;
	num2=15；
	console.log(num1);//15
	console.log(num2);//5

## 引用类型

	json，functio，object，null
	改变的是指针
	每一个值不是独立的，因为是改变的是指针，修改修改一个改变全部

    var obj1 = new Object();
    obj1.name="tom";
    var obj2 = obj1;
    obj2.name = "jack";
    obj2.age=18;
    console.log(obj1.name);  //jack
    console.log(obj2.name);  //jack
    console.log(obj1.age);   //18


## typeof的极限

检测引用类型的时候typeof都返回的object

	alert(person instanceof Object); // 变量 person 是 Object 吗？

	

推荐使用toString.call();


## 块级作用域

JavaScript 没有块级作用域


	for (var i=0; i < 10; i++){
		doSomething(i);
	}
	alert(i); //10

	如果是在 C、 C++或 Java 中， color 会在 if 语句执行完毕后被销毁。但在 JavaScript 中， if 语句中的变量声明会将变量添加到当前的执行环境（在这里是全局环境）中


## 垃圾收集

JavaScript 具有自动垃圾收集机制，找出那些不再继续使用的变
量，然后释放其占用的内存。为此，垃圾收集器会按照固定的时间间隔（或代码执行中预定的收集时间），周期性地执行这一操作


### 标记清除（mark-and-sweep）

变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”。当变量离开环境时，则将其标记为“离开环境”。垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾收集器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。


### 引用计数（reference counting）

引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是 1。如果同一个值又被赋给另一个变量，则该值的引用次数加 1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减 1。当这个值的引用次数变成 0 时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。


缺陷：循环引用

	var element = document.getElementById("some_element");
	var myObject = new Object();
	myObject.element = element;
	element.someObject = myObject;

变量 myObject 有一个名为 element 的属性指向 element 对象；而变量 element 也有一个属性名叫 someObject 回指 myObject。由于存在这个循环引用，即使将例子中的 DOM 从页面中移除，它也永远不会被回收。

解决：手动释放

	myObject.element = null;
	element.someObject = null;


## 内存分配

分配给 Web浏览器的可用内存数量通常要比分配给桌面应用程序的少。这样做的目的主要是出于安全方面的考虑，目的是防止运行 JavaScript 的网页耗尽全部系统内存而导致系统崩溃。


确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为 null 来释放其引用



	function createPerson(name){
		var localPerson = new Object();
		localPerson.name = name;
		return localPerson;
	}
	var globalPerson = createPerson("Nicholas");
	// 手工解除 globalPerson 的引用
	globalPerson = null;


# length的妙用


数组的 length 属性很有特点——它不是只读的。因此，通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项。


	var colors = ["red", "blue", "green"]; // 创建一个包含3个字符串的数组
	colors.length = 2;
	alert(colors[2]); //undefined


利用 length 属性也可以方便地在数组末尾添加新项：


	var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
	colors[colors.length] = "black"; //（在位置 3）添加一种颜色
	colors[colors.length] = "brown"; //（在位置 4）再添加一种颜色


由于数组最后一项的索引始终是 length-1，因此下一个新项的位置就是 length

	var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
	colors[99] = "black"; // （在位置 99）添加一种颜色
	alert(colors.length); // 100
	位置 3 到位置 98 实际上都是不存在的，所以访问它们都将返回 undefined



----------
# Array 

## 重排序方法

	reverse()   //反序
	sort()		//升序排列数组项——即最小的值位于最前面，最大的值排在最后面

sort会调用每个数组项的 toString()转型方法，比较的是字符串大小，原本的方法并不准确需要自己修改，收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回 0，如果第一个参数应该位于第二个之后则返回一个正数.


	arr.sort(compare);
	function compare(value1, value2) {  //这个方法可以用于数值的排序
	    console.log(value1,value2);  //会用冒泡排序的方式一个一个比较arr的值
	    if (value1 < value2) {
	        return -1;    //为负就说明，value1在2的前面
	    } else if (value1 > value2) {
	        return 1;     //为正就说明，value1在2的后面
	    } else {
	        return 0;   //为0就说明，value1=value2
	    }
	}

	function compare(value1, value2){   //最简单的数值型从小到大的排序
		return value2 - value1;
	}

## 筛选 filter()

	var numbers = [1,2,3,4,5,4,3,2,1];
	var filterResult = numbers.filter(function(item, index, array){
	return (item > 2);
	});
	alert(filterResult); //[3,4,5,4,3]


# Date

开始：

1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期

格式：

 YYYY-MM-DDTHH:mm:ss.sssZ（例如 2004-05-25T00:00:00）

日期格式化方法：


	toDateString()——以特定于实现的格式显示星期几、月、日和年；
	
	toTimeString()——以特定于实现的格式显示时、分、秒和时区；
	
	toLocaleDateString()——以特定于地区的格式显示星期几、月、日和年；
	
	toLocaleTimeString()——以特定于实现的格式显示时、分、秒；
	

----------


# Function


**函数名本身就是变量，所以函数也可以作为值来使用**

## 声明方式

函数声明与函数表达式的区别：

解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）

至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行


	alert(sum(10,10));
	function sum(num1, num2){   //函数声明，执行前被提升到调用的前面
		return num1 + num2;
	}

	alert(sum(10,10));
	var sum = function(num1, num2){  //函数表达式，没有被提前，报错
		return num1 + num2;
	};


	var 变量名 = new Function("参数1","参数2",...,"参数n","函数体");  
	//Function构造函数，不推荐这种语法会导致解析两次代码，影响性能

	var sum = function sum(){} //Safari 中会导致错误

## 固有属性


###  arguments 保存函数参数，伪数组

 callee 该属性是一个指针，指向拥有这个arguments对象的函数，用于解耦合

	function factorial(num){
		if (num <=1) {
			return 1;
		} else {
			return num * factorial(num-1)
		}
	}


	function factorial(num){
		if (num <=1) {
			return 1;
		} else {
			return num * arguments.callee(num-1)
		}
	}
		
### this

指向函数调用对象，重点关注()的那一块谁调用就是谁，全局作用域下为window


# Object

## 对象属性方法的权限操作

属性：

	[[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特
	性，或者能否把属性修改为访问器属性。默认true
	把 configurable 设置为 false，表示不能从对象中删除属性，一旦把属性定义为不可配置的，
	就不能再把它变回可配置了。

	[[Enumerable]]：表示能否通过 for-in 循环返回属性。默认true

	[[Writable]]：表示能否修改属性的值。默认为true

	[[Value]]：包含这个属性的数据值，默认undefined


方法：


	[[Get]]：在读取属性时调用的函数。默认值为 undefined。

	[[Set]]：在写入属性时调用的函数。默认值为 undefined。

eg1：

	var person = {};
	Object.defineProperty(person, "name", {  //IE8及以下不推荐使用
		writable: false,   //不可改
		value: "Nicholas"	//默认为Nicholas
	});
	alert(person.name); //"Nicholas"
	person.name = "Greg";
	alert(person.name); //"Nicholas"


	var person = {};
	Object.defineProperty(person, "name", {   //IE8及以下不推荐使用
		value: "Nicholas"
	});
	alert(person.name); //"Nicholas"
	delete person.name;
	alert(person.name); //"Nicholas"


eg2：  修改属性   //ie9支持

	var book = {
		_year: 2004,   //用于判断年份，不用于输出
		edition: 1
	};
	Object.defineProperty(book, "year", {
		get: function(){
			return this._year;
		},
		set: function(newValue){  //book.year = 2005送过来的值
			if (newValue > 2004) {
				this._year = newValue;
				this.edition += newValue - 2004;
			}
		}
	});
	book.year = 2005;   //因为defineProperty的是year不是_year
	alert(book.edition); //2


## 对象的创造


### 构造函数


	构造函数：

	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function(){
		alert(this.name);
		};
	}

	特点：
	-应该注意到函数名 Person 使用的是大写字母 P。用于区分构造函数和一般函数
	-没有显式地创建对象
	-直接将属性和方法赋给了 this 对象
	-没有 return 语句


	实例：
	var person1 = new Person("Nicholas", 29, "Software Engineer");
	var person2 = new Person("Greg", 27, "Doctor");

	new的实际步骤：
	(1) 创建一个新对象；
	(2) 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
	(3) 执行构造函数中的代码（为这个新对象添加属性）；
	(4) 返回新对象。


	var obj={};     //创建一个新对象
    obj.__proto__ = Base.prototype; //__proto__指向原型,copyBase原型的方法属性
	Base.call(obj);  //this指向obj,并且把Base构造函数的方法属性都借给了obj
	return obj；    //返回新对象




	person1.constructor == Person   //true 用于查看构造函数


### new的本质


	function _new(OBJ) {
	    var obj={};
	    obj.__proto__=OBJ.prototype;  //copy原型的属性和方法
	    OBJ.call(obj);                //copy构造的属性和方法
	    return obj;
	}
	
	var person=_new(Person);
	console.log(person);




**构造函数与其他函数的唯一区别，就在于调用它们的方式不同**

**任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数；而任何函数，如果不通过 new 操作符来调用，那它跟普通函数也不会有什么两样**


	缺点：
	就是每个方法都要在每个实例上重新创建一遍


### 原型模式

	Person.prototype = {
		name : "Nicholas",
		age : 29,
		job: "Software Engineer",
		sayName : function () {
		alert(this.name);
		}
	};


**每个函数都有一个 prototype（原型）属性，这个属性是一个指针，指向一个对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法**

这里需要理解，原型prototype对象，是在对象在被创建时候被创建的。不是原型一开始就存在，对象clone原型出来的。prototype对象用于存储实例共享的属性和方法。所有原型对象都会自动获得一个 constructor（构造函数）属性，指向构造函数。


**创建了自定义的构造函数之后，其原型对象默认只会取得 constructor 属性；至于其他方法，则
都是从 Object 继承而来的。**


当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性--__proto__），指向构造函数的原型对象。

注意__proto__存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间。


	

	function Person(){
	}
	Person.prototype.name = "Nicholas";
	Person.prototype.age = 29;
	Person.prototype.job = "Software Engineer";
	Person.prototype.sayName = function(){
		alert(this.name);
	}

	这里Person构造函数为空对象，原型是后面添加出来的对象


	实例的__proto__指向构造函数的原型对象

	var person=new Person();

	person.__proto__===Person.prototype



这里需要注意：


虽然可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值



	function Person(){
	}
	Person.prototype.name = "Nicholas";
	};
	var person1 = new Person();
	var person2 = new Person();
	person1.name = "Greg";
	alert(person1.name); //"Greg"—— 来自实例
	alert(person2.name); //"Nicholas"—— 来自原型




`hasOwnProperty()`用于检测一个属性是存在于实例中，只在给定属性存在于对象实例中时，才会返回 true

	
	alert(person1.hasOwnProperty("name")); //false
	

`in`用于检测属性是否存在于对象中（包括构造函数和原型）


	alert("name" in person1); //true


` Object.keys()`用于查看实例有什么可以枚举的属性和方法，不包含原型   ie9+


	var keys = Object.keys(Person.prototype);
	alert(keys); //"name,age,job,sayName"



实例的先后顺序对有影响


	function Person(){
	}
	var friend = new Person();
	Person.prototype = {    //重写原型的缺陷是constructor指向不对，会新建一个原型对象
		constructor: Person,   //需要特意修正
		name : "Nicholas",
		age : 29,
		job : "Software Engineer",
		sayName : function () {
			alert(this.name);
		}
	};
	friend.sayName(); //error


因为{}建立的同时就会创建一个原型对象，这样就 constructor 的指向就变成了object了


实例后改写原型会新建一个原型对象,切断了现有原型与任何之前已经存在的对象实例之间的联系，它们引用的仍然是最初的原型，所以说会报错。

避免的方法是

一：

	function Person(){
	}
	Person.prototype = {
	    constructor: Person,
	    name : "Nicholas",
	    age : 29,
	    job : "Software Engineer"
	};
	var friend = new Person();

二：
	function Person(){
	}
	Person.prototype = {
	    constructor: Person,
	    name : "Nicholas",
	    age : 29,
	    job : "Software Engineer"
	};
	var friend = new Person();
	Person.prototype.sayName=function () {  //这样写没有建立新的对象自然没有新的原型
	    alert(this.name);
	};
	friend.sayName(); //
	

#  继承

##  原型链


原型链是实现继承的主要方法

基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法。





son 继承了 father,son继承了father的方法和属性,构造函数继承构造函数，原型继承原型。属性的访问顺序依旧是按照访问机制来的。先构造再原型

	son.prototype = new father();


本质：son的原型指向了father，son的原型继承了，father的所有方法和属性。根据访问机制，现实找son的构造函数，再找son的原型，所以对于son构造函数拥有的属性和方法，并不能继承。

	    son.prototype.__proto__=father.prototype;  
	    son.prototype.call(father);  //son的原型copy了father所有的方法，改变this指向
	    return son.prototype;


继承的使用：对于祖传的方法，都丢在father的prototype中，只要father添加了，son就跟着有了，当然要使用`father.prototype[a]="ooxx";`的方式，不然原型链有问题



同时要注意，因为son的原型被father代替了，所以son原本原型中的方法，会被抹去，因此son单独的方法应该写在继承的后面。

错误：

	son.prototype.a=function(){
    	alert(233);
	};
	son.prototype = new father();    //这样方法a就没有了


正确：

	son.prototype = new father();
	son.prototype.a=function(){   //被覆盖了以后再
    	alert(233);
	};

## 使用call和apply

供爷法则，借东西的人在前面


	function father(name){
	    this.name = name;
	}
	function son(){
		//继承了 father，同时还传递了参数
	    father.call(this, "son");
	    this.age = 29;
	}
	var instance = new son();
	alert(instance.name);

缺点：方法都得写在构造函数中


## 推荐组合继承

原型链和借用构造函数的技术组合到一块。
原型链————将属性和方法都存在原型中，这样属性改一个其他都改
call和apply————将属性和方法都存在构造函数中，每次方法都是新建
结合在一起：
属性继承在构造函数中，方法继承在原型中


	function father(name){
	    this.name = name;
	    this.colors = ["red", "blue", "green"];
	}
	father.prototype.sayName = function(){
	    alert(this.name);
	};
	function son(name, age){
	    father.call(this, name);  //继承构造函数属性,color和name
	    this.age = age;           //age是自己的
	}
	//继承方法
	son.prototype = new father();  //son.prototype的复制father所有的属性，_proto__指向father复制father的方法
	son.prototype.constructor = son;  //修正son原型的constructor指向为son,之前指向的是father



## 基础知识


	原型对象是每个对象拥有的属性（原型），是一个对象。包含所有实例共享的属性和方法。
	prototype是构造函数指向原型对象的属性
	constructor是原型对象指向构造函数的属性
	__proto__是实例指向构造函数的原型对象的属性，而不是实例指向构造函数之间的属性 


## 属性的访问机制


![](http://b364.photo.store.qq.com/psb?/V11gSCnK0POvWR/fbyAPNJ7lN9b7HOgqyPwor2nyHXnngnRVIZHLMW4J4k!/b/dGwBAAAAAAAA&bo=lgOAAgAAAAADBzU!&rf=viewer_4)


1.现在构造函数对象中寻找

2.如果没有，到该对象的__proto__指向的对象继续寻找属性和方法

3.一直循环到__proto__为null


	Object.prototype.__proto__===null
	Function.prototype.__proto__===Object.prototype
	Function/Object/Aarray/String.__proto__===Functio.prototype


面向对象是通过_proto_实现的


	Function.prototype.__proto__指向Object.prototype	

	Object._proto_指向Function.prototype
	
	object.prototype._proto_指向null
总结：

Object——Function.prototype——Object.prototype——null

object是Function原型的实例，Function原型是object原型的实例

所以：所有对象Object，Array，Date，Number对象都是Function的实例

内置的关系图

第一代  

	Obeject.prototype


第二代

	Function


第三代：
	
	Object，Array，Date，Number，error


第四代（实例）：

	obj.__proto_===Object.prototype  
	
	arr.__proto_===Array.prototype
	
	reg.__proto_===RegExp.prototype
	
	data.__proto_===Date.prototype
	
	error.__proto_===Error.prototype


自创对象关系

第一代

	Obeject.prototype

第二代

	Function

第三代

	fn

第四代

	new fn


原型链的关系图：


![](http://i.imgur.com/pNqiapr.png)



#  label的作用


    <label>
        <input id="checkbox" type="checkbox">
        <span>123123</span>
    </label>


点击span可以触发input的事件


	

	


