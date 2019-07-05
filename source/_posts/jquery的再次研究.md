---
title: jquery的再次研究
date: 2018-01-17 16:45:20
categories: jquery
---


# 前言

公司里jq用的比较多，基本功能大体都知道了，但是代码比较冗杂。特意研究一下精简一下自己的代码





<!--more--><!--more-->


# jq简介


1.jquery是一个轻量级的库。

2.优点：

	1.选择器强大
    2.事件处理器可靠
    3.兼容性好
    4.链式操作

等等

3.jquery是一个闭包函数，不污染顶级变量


# 入口函数

   1.dom结构绘制完毕就执行，原生要所有内容加载完毕才行包括图片；
   2.一个页面可以有多个，原生只能有一个；
   
  	$(function(){})


# 区分DOM对象和$对象
  
  原生dom对象：通过原生方法获取的dom对象，或者$循环出来的；只能用原生的方法

  $对象就是：$(dom对象)；只能使用jq的方法


 转化：    

	 ob=$ob[0];      ob=$ob.get(0);
            
     $ob=$(ob);


# jq与其他库冲突


  var $J=jQuery.noConflict();  //自定义的快捷方式

  $J(function(){});


# 选择器

  css中的选择器全都可以用


## 表单选择器

  $(":text")   $(":password")


## 选择器中含有特殊字符


要使用转义符

  $("#id、、#b")


## 选择器空格问题


  对于一个有多个class名的要用属性选择器或类名选择器  要注意类名写全否则可能选不到







# bug注意点


## attr()改变checkbox的checked无效

描述：attr改变checked、disabled等功能的时候发现，第一次点击有效，之后点击无效。


原因：因为checked属于为原型对象的属性。而attr在remove原型对象时会出错。原型对象指的是自身自带的，无法移除。


解决：在jquery 1.6之后的版本中，改变元素的boolean类型属性用prop才能生效，而其他类型属性则继续沿用attr;

 	$("input[name=ids]").prop("checked", true);





# 源码


# 其中使用过的判断和正则

## 选择器部分

###  输入的元素是<XXX>型


	selector.charAt(0) === "<" && 
	//第一个元素是< 

	selector.charAt( selector.length - 1 ) === ">" && 
	//最后一个元素是> 

	selector.length >= 3 
	//<>之间必须有内容  


### 防止利用hash在注入XSS

	rquickExpr = /^(?:\s*(<[\w\W]+>)[^>]*|#([\w-]*))$/

	^字符串的开始位置  $字符串的结尾位置

	?匹配前面的子表达式零次或一次，或指明一个非贪婪限定符

	?:代表不捕获分组   \s匹配任何空白字符   *前面的子表达式零次或多次

	[]表达式的开始   +前面的子表达式一次或多次

    -除换行符 \n 之外的任何单字符

	\w表示字符类（包括大小写字母，数字）
	\W表示非字符类（符号的内容）

	?:\s 不捕获空格多次或者0次

	(<[\w\W]+>) <XXX...>  XXX为所有内容

	[^>]*  >或者>>>...  

	#([\w-]*)   #XXX...（除了回车）多个

总体是说：


	<XXX...>>>>  ||  <XXX....>#XXX....

	
### 匹配空标签

	rsingleTag = /^<(\w+)\s*\/?>(?:<\/\1>|)$/,


	^字符串的开始位置  $字符串的结尾位置

	?匹配前面的子表达式零次或一次，或指明一个非贪婪限定符

	?:代表不捕获分组   \s匹配任何空白字符   *前面的子表达式零次或多次

	[]表达式的开始   +前面的子表达式一次或多次

    -除换行符 \n 之外的任何单字符

	\w表示字符类（包括大小写字母，数字）
	\W表示非字符类（符号的内容）

	?:\s 不捕获空格多次或者0次

	 

## exec()的使用


	match = rquickExpr.exec( selector );

exec() 方法用于检索字符串中的正则表达式的匹配。

	RegExpObject.exec(string)

返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。
	数组第一个为正则表达式相匹配的文本；
	index 属性声明的是匹配文本的第一个字符的位置；
	input 属性则存放的是被检索的字符串 string


	console.log(/w/g.exec("hello world"));

	["w", index: 6, input: "hello world"]


    ?:的分析

	要在一篇文章中查找"program"和"project"两个单词，正则表达式可表示为/program|project/,也可表示为/pro(gram|ject)/，但是缓存子匹配(gramject)没有意义，就可以用/pro(?:gram|ject)/进行非捕获性匹配这样既可以简洁匹配又可不缓存无实际意义的字匹配。

	 console.log((/pro(gram|ject)/).exec("program"));

	["program", "gram", index: 0, input: "program"]


	console.log((/pro(?:gram|ject)/).exec("program"));

	["program", index: 0, input: "program"]

    不用捕获?:后面的东西




#  里面可以借鉴的方法

## 获取函数的传入的参数


  	 function Fn(){
		var target = arguments[0] || {} ；  //没传入参数就为{}空
		var length = arguments.length;   //参数的长度
	}


## 字符串转成数组在循环，生成json

	var class2type={}；
	
    jQuery.each("Boolean Number String Function Array Date RegExp Object Error".split(" "), function(i, name) {
        class2type[ "[object " + name + "]" ] = name.toLowerCase();
    });


## 判断输入的字符类型


    var class2type={};
    var core_toString = class2type.toString;

    function fn() {
       return null;
    }
    jQuery.each("Boolean Number String Function Array Date RegExp Object Error".split(" "), function(i, name) {
        class2type[ "[object " + name + "]" ] = name.toLowerCase();
    });
    function type( obj ) {
        if ( obj == null ) {
            return String( obj );
        }
        return typeof obj === "object" || typeof obj === "function" ?
            class2type[ core_toString.call(obj) ] || "object" :
            typeof obj;
    }

type of可以识别的类型：number、boolean、string、object、undefined、function

无法识别object里面更多详细的类型：Array 、Date 以及自定义类

	1.判断null和undefined

	2.剩下的再判断function和object，number、boolean、string一类

	3.function里面的返回值为false，null，‘’  这种情况



## 简单判断type

    function type(obj){
        return Object.prototype.toString.call(obj).slice(8,-1);
        //"[object Object]"  截取  第8个元素  和倒数第二个袁术
    }


## 判断是不是window对象


	isWindow: function( obj ) {
		return obj != null && obj === obj.window;
	}

	//obj != null   是为了防止报错   null.window 报错


## 判读是不是数字


	isNumeric: function( obj ) {
			return !isNaN( parseFloat(obj) ) && isFinite( obj );
	}



    //isFinite() 函数用于检查其参数是否是无穷大
    //如果 number 是有限数字（或可转换为有限数字），那么返回 true。
    // 否则，如果 number 是 NaN（非数字），或者是正、负无穷大的数，则返回 false。


    //NaN不能通过相等操作符（== 和 ===）来判断 ，NaN == NaN 和 NaN === NaN 都会返回 false
    //isNaN() 函数通常用于检测 parseFloat() 和 parseInt() 的结果，以判断它们表示的是否是合法的数字


## 判断是否为DOM节点

	if(obj.nodeType)

   //dom  节点都有nodeType属性


## 判断是不是纯粹的对象


基础：

    //hasOwnProperty()  方法对待自身属性和继承属性的区别
    // 防止hasOwnProperty被修改，使用原型链上真正的 hasOwnProperty 方法 ({}).hasOwnProperty.call(foo, 'bar'); // true
    //console.log(core_hasOwn);


    o = new Object();
    o.name = 'exists';
    console.log(class2type);
    console.log(o.hasOwnProperty('name'));              // 返回 true     自身的
    console.log(o.hasOwnProperty('toString'));          // 返回 false    继承的
    console.log(o.hasOwnProperty('hasOwnProperty'));    // 返回 false    继承的

    console.log(o);





正文：


	//判断指定参数是否是一个纯粹的对象。
	//所谓"纯粹的对象"，就是该对象是通过"{}"或"new Object"创建的
	isPlainObject: function( obj ) {
		// Not plain objects:
		// - Any object or value whose internal [[Class]] property is not "[object Object]"
		// - DOM nodes
		// - window





		if ( jQuery.type( obj ) !== "object" || obj.nodeType || jQuery.isWindow( obj ) ) {
            //＄.type 返回的不是object    ＤＯＭ节点　window对象以及的情况。
			//nodeType 属性返回以数字值返回指定节点的节点类型。
			// 如果节点是元素节点，则 nodeType 属性将返回 1。
			// 如果节点是属性节点，则 nodeType 属性将返回 2。
			return false;
		}

		// Support: Firefox <20
		// The try/catch suppresses exceptions thrown when attempting to access
		// the "constructor" property of certain host objects, ie. |window.location|
		// https://bugzilla.mozilla.org/show_bug.cgi?id=814622


		try {
			//只有Object.prototype中才有自带的“constructor”属性
			//以及Object.prototype中才有自带的“isPrototypeOf”属性
			if ( obj.constructor &&
					!core_hasOwn.call( obj.constructor.prototype, "isPrototypeOf" ) ) {
                //obj没有constructor  或者  没有     obj.constructor.prototype.hasOwnProperty(isPrototypeOf)
				return false;
			}
		} catch ( e ) {
			return false;
		}

		// If the function hasn't returned already, we're confident that
		// |obj| is a plain object, created by {} or constructed with new Object
		return true;
	},



## 判断是不是空对象



    function isEmptyObject( obj ) {
        var name;
        for ( name in obj ) {
            return false;
        }
        return true;
    }



## 伪数组转化为真数组


	Array.prototype.slice.call(arguments)能将具有length属性的对象转成数组


	Array.prototype.slice.call(arguments)

	[].slice.call(arguments)


	console.log([].slice.call({3:'li',1:'li',2:'li',length:3}));  //[empty, "li", "li"]


根据length来获取长度，然后根据符号的下标来转化


# 结构


21-94定义一些变量和函数

96-283给JQ对象，添加一些方法和属性

285-347   extend：JQ的继承方法

349-817   jQuery.extend（）；扩展一些工具方法

877-2856  sizzle:复杂选择器的实现 

2880-3042  callback：回调对象  函数的统一管理

3043-3183  deferred：延迟对象，对异步的统一管理

3184-3295  support：功能检查

3308-3652  data():数据缓存

3653-3797  queue（）：队列管理   动画的执行顺序啥的

3803-4299 attr()  prop()  val() 等 给元素属性的操作

4300-5128  on（） trigger（）   事件操作的相关方法

5140-6057   dom操作  增删查改 都没筛选

6058-6620 css)()  样式的操作  

6621-7854  提交的书记和ajax()的操作

7855-8584  animate（）  运动的方法


8585-8792  offset()  位置和尺寸的方法

8804-8821   jq支持模块化的模式

8826    输出接口       window.jQuery=window.$=jQuery;


# 框架

    (function( window, undefined ){
         var jQuery=function() {
          console.log(1);
        };
        window.jQuery=window.$=jQuery;
    })(window);
    jQuery();


1.闭包函数，避免污染全局变量。


2.为什么要传window

	1 查找速度很多，不然找全局window根据机制每次都要找一段时间

	2 压缩的时候很有用


3.为什么要传undefined

	防止undefined在外面进行修改


4.为什么不用严格模式  

	不推荐，防止出现兼容性问题

2.jq是一个面对对象的写法(实例方法)

     源码：
     jQuery = function( selector, context ) {   
         //返回的是jq的对象
         return new jQuery.fn.init( selector, context, rootjQuery );
     }




	  $("#div").css();
	
	  var arr=new  Array();
      arr.push();


3.扩展工具方法


    $.ajax();


#  变量

## rootjQuery


jquery的根目录，和jQuery(document)同样

赋予的原因：

    1 方便压缩
    2 使用变量方便维护，看名字知道干什么
   


## readyList


## core_strundefined

存的undefined的字符串

用于防止xml的dom判断的时候存在出错的bug

	core_strundefined = typeof undefined,



## location document docElem

将其进行存储，这样有利于压缩


## _jQuery  _$

防止外部的jQuery和$被占用


## class2type

$.type(); 判断类型

## core_deletedIds

已经 删除




# 无new写法


	jQuery = function( selector, context ) {
		return new jQuery.fn.init( selector, context, rootjQuery );
	}

	jQuery.fn = jQuery.prototype={
       return this；
	}
	jQuery.fn.init.prototype = jQuery.fn;

思路因为jq是无new构造，一般的方法会无线new，这个时候用的思路就是在jq建立一个新的函数然后继承其功能，然后返回其即可


# 给JQ添加对象和方法


## jquery 版本

## constructor 指向修正

	//修正原型的指向，防止被外部修改     jQuery.prototype.constructor=jQuery


	jQuery.prototype.name=30;  constructor-jQuery
     
    jQuery.prototype={name:30}; constructor-Object


 防止被修改

## init 初始化和参数的提供


### 字符串选择器的思路


![](https://i.imgur.com/N1aQmiO.png)

	因为$('li')和css（）是两种不同的方法

	获取的数组不能在两个作用域中使用，所以利用this来实现两个函数的连接
      


	<ul>
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	</ul>

    console.log($('li').css('background-color','red'));



选择器的逻辑：


    if（str为空的时候）  返回this


    if（str是字符串的时候）{

		if(str是<XXX...>){
			match

		}else{

			match
		}
				
	}


3.扩展工具方法


    $.ajax();


##  变量

### rootjQuery


jquery的根目录，和jQuery(document)同样

赋予的原因：

    1 方便压缩
    2 使用变量方便维护，看名字知道干什么
   


### readyList


### core_strundefined

存的undefined的字符串

用于防止xml的dom判断的时候存在出错的bug

	core_strundefined = typeof undefined,





