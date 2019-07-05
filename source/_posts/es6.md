---
title: '''es6'''
date: 2018-03-01 10:18:23
tags:
---



# 变量声明


var声明变量存在变量提升问题，即无论函数在哪里声明都会被放在全局作用域的顶部。


<!--more--><!--more-->

	  function aa() {
	    if(bool) {
	        var test = 'hello man'
	    } else {
	        console.log(test)
	    }
	  }

	function aa() {
	    var test // 变量提升
	    if(bool) {
	        test = 'hello man'
	    } else {
	        //此处访问test 值为undefined
	        console.log(test)
	    }
	    //此处访问test 值为undefined
	  }

块级作用域： {}大括号内的代码块即为let 和 const的作用域，他们的作用域是在它所在当前代码块，但不会被提升到当前函数的最顶部。

## const

常量，块级作用域

    const name = 'lux'
    name = 'joe' //再次赋值此时会报错




## let

变量，块级作用域


	  function aa() {
	    if(bool) {
	       let test = 'hello man'
	    } else {
	        //test 在此处访问不到
	        console.log(test)
	    }
	  }


面试题：


	var funcs = []
    for (var i = 0; i < 10; i++) {
        funcs.push(function() { console.log(i) })
    }
    funcs.forEach(function(func) {
        func()
    })；
	//全是10   因为没有保存i当时的值 全是在i=10时执行


# 字符模板

``表示模板 
 
- ${}表示变量
- 空格、新行、缩进，都会原样输出在生成的字符串
- 可以在内部使用标签
- 不支持条件语句

    //es5 
    var name = 'lux'
    console.log('hello' + name)
    //es6
    const name = 'lux'
    console.log(`hello ${name}`) //hello lux


# 字符串函数

1.includes：判断是否包含然后直接返回布尔值

    let str = 'hahay'
    console.log(str.includes('y')) // true

 2.repeat: 获取字符串重复n次

    let s = 'he'
    console.log(s.repeat(3)) // 'hehehe'
    //如果你带入小数, Math.floor(num) 来处理


# 函数


## 提供了默认值

ES5

    function action(num) {
        num = num || 200
        //当传入num时，num为传入的值
        //当没传入参数时，num即有了默认值200
        return num
    }


ES6:


    function action(num = 200) {
        console.log(num)
    }
    action() //200
    action(300) //300

