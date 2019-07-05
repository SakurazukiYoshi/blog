---
title: jq源代码
date: 2018-02-02 16:38:15
categories: jquery
---




# 前言

----------


对于工作中最常使用的js库，研究一下其他的源代码，为了以后的搭框架铺好路。


<!--more--><!--more-->




# 无new构建

思路：

- 因为jq是无new构造，一般的方法会无限new
	
	    var a=function () {
	        return new a();
	    };

- 解决jq原型上建立一个新的子函数，然后继承其功能，然后返回new 子函数即可。


	    !(function () {
	        var a=function () {
	            return new a.prototype.init();
	        };
	        a.prototype={
	            constructor:"a",
	            init:function () {
	                alert(1);
	                return this;
	            },
	            fn1:function () {
	                console.log(2)
	            }
	        };
	        a.prototype.init.prototype=a.prototype;
	        a.fn=function () {
	           console.log(1);
	           return this;
	        };
	        window.a=a;
	    })(window,undefined);
	
	    a.fn();  //直接执行的a.fn()  没有返回new a.prototype.init()  所以不用构建整个a
	    a().fn1();  //执行了一次a()   new a.prototype.init()   拷贝了大量的数据



这样做的原因：提高 jQuery 的查询效率。

jQuery选择器，根据传入的数据不同而有不同的jQuery对象。

jQuery的方法分为两种：


jq自带的方法：

	- 静态方法
	就是直接通过$访问的方法，这些方法一般不对dom元素操作，提供了一些常用的工具。
	eg：$.ajax,$.type,$.merge   
	
	-实例方法
	
	会对jQuery查询的DOM元素进行操作，jQuery 执行$()会构建一个 jQuery 对象，这个对象以数组的方法存储查询出的所有DOM元素，然后在这个对象的原型链上实现了对这些 DOM 操作的方法。
	eg:$().each(),$().html,$().remove()。


对于实例方法，因为要对DOM进行操作所以每个实例的jQuery的对象不一定相同，因此每个实例方法都需要new一次。

静态方法不需要对DOM操作每个对象都是一样的，因此没必要每次都要new一次存在jQuery的原型上即可。



jQuery.fn 中的方法为什么不直接定义在 init prototype上，而要定义在 jQuery prototype上？


	目的是为了提高 jQuery 的查询效率。
	
	直接定义在 init 的 prototype 对象上每执行一次查询（实例化一次），就会在内存中创建这样一个庞大的prototype对象，
	
	定义在jQuery prototype上，在jQuery加载时，这个对象就会被初始化并一直存在于内存中，以后每次执行 $() 时，只需要将init中的prototype指向这个对象就可以了，而不用每次都去创建一遍相同的对象。
	
	
	init 函数中返回的 this -- jQuery原型  //动态方法
		
	jQuery函数中的this   -- jQuery   //静态方法



- jQuery每次查询会创建一个jQuery对象，
- 所有 jQuery 对象都会共享同一个 jQuery原型。
- jQuery对象不仅包含了DOM查询结果集，还继承了 jQuery原型对象上的操作方法。
- 可以在查询后直接调用方法来操作这些DOM元素了。


缺陷：查询复杂，每次都要创建一个复杂的jQuery对象，尽管共享同一个 jQuery 原型，既要考虑各种不同的匹配标识，同时又要考虑不同浏览器的兼容性。如果是简单的查询不推荐使用jQ对象

















# 原型上自带的方法


## 目录




1. jquery:版本号
2. constructor:修正`jQuery.prototype={}`带来的constructor指向错误
3. init:初始化
4. selector:选中的元素
5. length:选中DOM中元素的个数
6. toArray:把DOM元素转换为数组，然后输出该数组元素的HTML
7. get:没带参数-$(OX)伪数组转化为真数组，带正参数-数组中的对应，负参数-length-参数的元素
8. pushStack：$(OX)选中记录的数组
9. merge:合并两个数组，修改第一个参数的内容


## toArray

	console.log(toString.call($("li")));        //[object Object]
	console.log(toString.call($("li").toArray()));  //[object Array]

jq选择器的DOM是一个假数组对象

   {0:'li',1:'li',2:'li',length:3,...}

转化为真数组：

	[li, li, li]


原理：

	[].slice.call(伪数组)；

	[].slice.call({0:'li',1:'li',2:'li',length:3})；



Array.prototype.slice.call(arguments)能将具有length属性的对象转成数组

	console.log([].slice.call({3:'li',1:'li',2:'li',length:3}));  
	//[empty, "li", "li"]


根据length来获取长度，然后根据符号的下标来转化


## pushStack


jQuery对象栈:每个遍历会找到一个jQuery对象，然后jQuery会把这组元素推入到栈中。

eg：

	$('ul').children('li').css("backgroundColor","#CCC");  

ul首先入栈，子元素li后入栈


源代码：


	pushStack: function( elems ) {  
	  
	        // Build a new jQuery matched element set  
	        var ret = jQuery.merge( this.constructor(), elems );  
	  
	        // Add the old object onto the stack (as a reference)  
	        ret.prevObject = this;  
	        ret.context = this.context;  
	  
	        // Return the newly-formed element set  
	        return ret;  
	}  

	merge: function( first, second ) {
		var l = second.length,
			i = first.length,
			j = 0;
	
		if ( typeof l === "number" ) {
			for ( ; j < l; j++ ) {
				first[ i++ ] = second[ j ];
			}
		} else {
			while ( second[j] !== undefined ) {
				first[ i++ ] = second[ j++ ];
			}
		}
	
		first.length = i;
	
		return first;
	}


	eq: function( i ) {
		var len = this.length,   //现在选中元素的长度
			j = +i + ( i < 0 ? len : 0 );  //i<0  len-i     i>0   len+i

		// i的个数超过选中的范围 pushStack([])     没超过就 pushStack(this[j])
		//一般是选中的html，即原生html对象
		return this.pushStack( j >= 0 && j < len ? [ this[j] ] : [] );
	}



先理解merge的作用：

- 将两个数组合并

	 $.merge( [0,1,2], [2,3,4] );  //[0,1,2,2,3,4]

- 将伪数组对象合并，只能合并length以内的数字key，其他的属性不能合并


		var a={
			0:'ul',
			context:'document',
			length:1
		};
		var b={
			0:'li',
			context:'html',
			length:1
		};
		merge(a,b);
		结果：
		var b={
			0:'ul',
			1：'li'
			context:'html',
			length:2
		};


在思考pushStack：


		

- this.constructor 就是jQuery的构造函数init,及是jQuery，所以this.constructor()返回一个jQuery对象.

- 构建一个新的jQuery对象，无参 this.constructor()，只是返回引用this

- jQuery.merge 把elems节点，合并到新的jQuery对象


- 给返回的新jQuery对象添加属性prevObject，所以我们看到prevObject 其实还是当前jQuery的一个引用罢了













# extend方法

----------




## extend的用法


`jQuery.extend`————把两个或者多个对象合并到第一个对象当中

`jQuery.fn.extend`———把对象挂载到 jQuery的prototype 上以扩展一个新的 jQuery 实例方法 




语法：

	jQuery.extend([deep,] [target,] object1 [,objectN]);

	jQuery.fn.extend([deep，] [target,] object1 [,objectN])




- deep: Boolen，选，表示是否进行递归合并（深/浅复制）默认浅复制false。



- target:扩展对象，选，将接收新的属性。



- objectN:一个对象，包含额外的属性，扩展到目标对象（扩展对象）。



案例：

	function getOpt(target, obj1, obj2, obj3){
	    $.extend(target, obj1, obj2, obj3);
	    return target;
	}
	
	var _default = {
	    name : 'wenzi',
	    age : '25',
	    sex : 'male'
	};
	var obj1 = {
	    name : 'obj1'
	};
	var obj2 = {
	    name : 'obj2',
	    age : '36'
	};
	var obj3 = {
	    age : '67',
	    sex : {'error':'sorry, I dont\'t kown'}
	};
	console.log(getOpt(_default, obj1, obj2, obj3));
	// {name: "obj2", age: "67", sex: {error: "sorry, I dont't kown"}}



## 区分深浅拷贝：



浅拷贝：

浅复制对象A时，对象B将复制A的所有字段：


- 如果字段是引用类型，B将复制地址


- 如果字段是基本类型，B将复制其值


缺点： 引用类型两个的地址一样，改变了B的地址，同样就改变了A的地址


	
	function copy(target,clone){
	    for(var i in clone){
	        target[i]  = clone[i];
	    }
	    return target;
	}

深拷贝：

完全复制所有数据


- 优点是B与A不会相互依赖（A,B完全脱离关联）


- 缺点是复制的速度慢，代价大。


		    function type(obj){
		        return Object.prototype.toString.call(obj).slice(8,-1);
		        //"[object Object]"  截取  第8个元素  和倒数第二个袁术
		    }
		    function deepCopy(target,cloneObj){
		        var copy;
		        for(var i in cloneObj){
		            copy = cloneObj[i];
		
		            //跳过循环中的一个迭代   防止a={b:a} 这种无限循环    构造函数不能喝之中的key相同
		            if(target === copy){
		                continue;
		            }
		/*
		* arguments.callee的作用：当函数被调用时，它的arguments.callee对象就会指向自身
		*
		*1. 这个属性只有在函数执行时才有效
		*
		*2. 它length属性，用来获得形参的个数，可以比较形参和实参个数是否一致，arguments.length和arguments.callee.length
		*
		*3. 它可以用来递归匿名函数。
		*
		* arguments.callee为自身    arguments.callee()  自身又执行了一次
		*
		* 现在已经不推荐使用arguments.callee()；
		* 缺点：因为它是个很大的对象，每次递归调用时都需要重新创建。影响现代浏览器的性能，还会影响闭包。
		*
		* */
		
		            if(type(copy) === "Array"){
        /*
        * copy一个  key:value  中value为数组
        *
        * value存在不存在
        * 存在     deepCopy(value,copy)
        *
        * 不存在    deepCopy([],copy)
        *
        * */
		                target[i] = arguments.callee(target[i] || [],copy);
		            }else if(type(copy) === "Object"){
        /*
        * copy={ c:"c" }    i=a   target[a]不存在
        *
        * deepCopy({},{ c:"c" } )    这里target[i]={}  重新copy
        *
        *
        * */
		                target[i] = arguments.callee(target[i] || {},copy);
		            }else{
		                target[i] = copy;      
		            }
		        }
		        return target;
		    }
		
		    var a = {
		        a:{ c:"c" },
		        b:"b"
		    };
		    var t = deepCopy({},a);
		    //t.a.c ="e";
		    console.log(t);//c
		    //console.log(a.a.c);//c


 深浅拷贝的实例区别



	var obj1 = {
	    name: "John",
	    location: {
	        city: "Boston",
	        county: "USA"
	    }
	}
	
	var obj2 = {
	    last: "Resig",
	    location: {
	        state: "MA",
	        county: "China"
	    }
	}
	
	$.extend(false, {}, obj1, obj2); 
	// { name: "John", last: "Resig", location: { state: "MA", county: "China" }}
	
	$.extend(true, {}, obj1, obj2); 
	// { name: "John", last: "Resig", location: { city: "Boston", state: "MA", county: "China" }}


 

- 深度复制 会递归遍历每个对象中含有复杂对象（如：数组、函数、json对象等）的属性值进行复制



- 浅度复制 只会选择最后一个对象的值。





## 代码思路




-  	第一个参数为布尔值的时候


记录传入的布尔值，目标对象变为第二个参数,循环+1

	jQuery.extend( true, obj1, obj2 );


- 传入的不是obj，也不是function的其他基本类型，其他的基本类型没有自身的属性

 		"hi".test="hello"   没效果    还是undefined


- 参数个数等于1,或者带布尔值的2个参数的情况


		jQuery.extend(obj)
		jQuery.extend(true，obj)


- 对参数开始循环


	1.获取参数对象的每个key，value

	2.将第一个参数的key和循环的key比较，防止自循

	3.循环的obj的value存在并且是一个纯对象，或者是一个数组

	3.1 数组和对象的异常情况分别用[]  {}处理



## jq插件的开发


源代码：


	if ( length === i ) {
		target = this;
		--i;
	}





这里首先的明白this的指向：

1. this的指向在函数定义的时候是确定不了的，
2. 只有函数执行的时候才能确定this到底指向谁，
3. 实际上this的最终指向的是那个调用它的对象
4. apply和call可以改变this指向。

返回规则：



- 如果返回值是一个对象，那么this指向的就是那个返回的对象


- 如果返回值不是一个对象那么this还是指向函数的实例




这里调用的是

1. Fn();调用

2. new关键字可以改变this的指向，将这个this指向f1.

3. F1没有返回值，指向函数的实例为fn


	    function Fn() {
	       console.log(this);
	    }
	
	    Fn();   //window
	
	    var f1=new Fn();   //Fn
	
	    /**
	     
	        function a() {
	            var f1={};
	            f1.__proto__==Fn.prototype;
	            Fn.call(f1);       //f1 的this 指向Fn
	            return f1;
	        }
	
	    * */



所以构造函数里的方法指向的this通常是指向的构造函数


    var a=function () {
        return new a.prototype.init()
    };

	a().say();   //  new a.prototype.init().say();

    a.say();    //  a在调用


所以原型内部的方法this是指向init,通过原型链找到`a.prototype`




jQuery插件开发分为两种：



	/*
	* jQuery.extend时，this指的是jQuery；
	* $.extend()的
	* $.ooxx=function(){}     调用$.ooxx();
	*
	* jQuery.fn.extend时，this指的是jQuery.fn
	* $.prototype=function(){ooxx:function(){}}  调用$().ooxx();
	*
	* */


本质就是利用`this`在不同情况下的指向不同，然后分别在jQuery和jQuery的原型上拷贝原型上面没有的方法。



## 事件绑定

### 事件的基础


1.历史  DOM事件的级别,对于DOM的规范版本

	DOM0  
		element.onclick = function(){}

    DOM2
  
	window.addEventListener('click', function () {...}, false);
	window.removeEventListener('click', listener, false);
    增减了三个事件模型：捕获阶段，目标阶段，冒泡阶段

	DOM3
    对于addEventListener增添了一些时间，
    
	DOM1中没有对于时间的修正


	dispatchEvent   vue的关键点

捕获流程：

捕获阶段：window-document-html-body-target

冒泡：反过来


DOM事件模型：IE所使用的冒泡型事件（Bubbling）+DOM标准定义的冒泡型与捕获型（Capture）的事件



事件代理的原理：

- 由于事件会在冒泡阶段向上传播到父节点
- 把子节点的监听函数定义在父节点上，利用e.target来获取触发的子节点，由父节点的监听函数统一处理多个子元素的事件。


好处是：

- 只要定义一个监听函数，就能处理多个子节点的事件
- 以后再添加子节点，监听函数依然有效。
- 如果希望事件到某个节点为止，不再传播，可以使用事件对象的stopPropagation方法


	event.currentTarget  //获得其监听器触发了事件的那个元素

	event.target    //获得触发事件的元素


后两者的区别：

	target:触发事件的某个具体对象，只会出现在事件流的目标阶段
	
	currentTarget:绑定事件的对象，恒等于this，可能出现在事件流的任意一个阶段中


	父子嵌套的关系中，父元素绑定了事件，单击了子元素，
	这时候currentTarget指向的是父元素，因为他是绑定事件的对象，
	而target指向了子元素，因为他是触发事件的那个具体对象

	事件委托的时候，两者的对象则不同  

实例：将li的事件绑定在了ul上，利用e.target来判断谁被点击了

需要注意的是这里：捕获阶段是到触发事件的节点为止，这里的触发节点并不是指绑定事件的节点，是指在到由windows到target节点这个阶段中发现事件的节点就执行

	<ul>
	    <li>第1个<button class="btn" id="1">删除</button></li>
	    <li>第2个<button class="btn" id="2">删除</button></li>
	    <li>第3个<button class="btn" id="3">删除</button></li>
	</ul>

    var ul=document.querySelector('ul');
    ul.addEventListener('click',function (e) {
        var liNow=e.target;
        liNow.innerHTML='hello world';
    },false);





### 绑定基础

    * .bind()
    * .live()
    * .delegate()
    * .on()


不管是用什么方式绑定,归根到底还是用addEventListener/attachEvent处理的


	document.addEventListener('click',function () {},false)


现有问题：

1. 大量的事件绑定，性能消耗，而且还需要解绑（IE会泄漏）
2. 绑定的元素必须要存在
3. 后期生成HTML会没有事件绑定，需要重新绑定
4. 语法过于繁杂


事件委托:可以解决第大量事件绑定的性能消耗

DOM2.0模型将事件处理流程分为三个阶段：

1. 事件捕获阶段
2. 事件目标阶段
3. 事件起泡阶段


事件委托就是事件目标自身不处理事件，而是把处理任务委托给其父元素或者祖先元素.

不管你用的是（click / bind / delegate)之中那个方法，最终都是jQuery底层都是调用on方法来完成最终的事件绑定


add  -4554
on  -  5256



	


