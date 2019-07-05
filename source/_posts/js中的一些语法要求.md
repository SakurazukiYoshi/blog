---
title: '''js中的一些语法要求'''
date: 2017-05-26 10:45:50
categories: css
---


# 简介

本文章用于记录一些js需要注意的语法



<!--more--><!--more-->



# 构造函数的首字母要大写

用于区分构造函数和一般函数

	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function(){
		alert(this.name);
		};
	}


----------


# _用于表示只能通过对象方法访问的属性

表明为私有属性，只有对象的内部才能使用


	var book = {
		_year: 2004,
		edition: 1
	};
	Object.defineProperty(book, "year", {
		get: function(){
		return this._year;
	},
	set: function(newValue){
		if (newValue > 2004) {
			this._year = newValue;
				this.edition += newValue - 2004;
			}
		 }
	});
	book.year = 2005;
	alert(book.edition); //2



----------


# +用于将string转化为数字

	var str=“1”；
	（+str>0）&&（alert(1)）;



----------


# a&&b&&c

	a&&c  相当于if（a）{b}

	a&&b&&c 相当于  if（a）{b；c}

这里需要注意的是位于中间的b必须为true，否则失效，当为function的时候尤为注意。


  	function(){}  没有返回值的时候，返回的是undefined，这样就为false；

所以中间为function的时候最好返回一个为true的值；



----------


# 对象的创建


	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function(){
		alert(this.name);
		};
	}
	Person.prototype = {   //这样写的缺陷是constructor失效，指向object了
		name : "Nicholas",
		age : 29,
		job: "Software Engineer",
		sayName : function () {
			alert(this.name);
		}，
		constructor : Person   //特意修正constructor的指向
	};

	var friend = new Person();   //new推荐写在下面，否则会肯能报错

# 继承的写法

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




# 函数的访问


访问函数的指针而不执行函数的话，必须去掉函数名后面的那对圆括号

	function fn(){
    	alert(1);
	}	
	console.log(fn);  //返回的是函数fn
	console.log(fn()); //返回的是undefined，因为函数执行了，没有返回值


所以是：

	document.getElementById("root").onclick=function(){
        console.log("hello world")
    }

而不是：

	document.getElementById("root").onclick=function(){
        console.log("hello world")
    }()；  这样直接就执行了

# 基本类型值的定义过程


	var s1 = "some text";
	var s2 = s1.substring(2);  


本质：用引用类型创建，然后在销毁，基本类型应该没有方法

	var s1 = new String("some text");
	var s2 = s1.substring(2);
	s1 = null;


	var s1 = "some text";
	s1.color = "red";
	alert(s1.color); //undefined   
这里没有是因为第二行创建的 String 对象在执行第三行代码时已经被销毁了。第三行代码又创建自己的 String 对象，而该对象没有 color 属性
