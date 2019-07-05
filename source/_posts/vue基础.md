---
title: vue基础
date: 2017-05-02 09:05:33
tags: css
categories: vue
---


#  渐进式框架

每个框架都不可避免会有自己的一些特点，从而会对使用者有一定的要求，这些要求就是主张，主张有强有弱，它的强势程度会影响在业务开发中的使用方式。


<!--more--><!--more-->


渐进式框架---主张最少

Angular-强主张的，如果你用它，必须接受以下东西：

- 必须使用它的模块机制
- 必须使用它的依赖注入
- 必须使用它的特殊形式定义组件（这一点每个视图框架都有，难以避免）


# 两个核心

- 双向数据绑定

原理  Object.defineProperties()的setter和getter


- 组合的视图组件


# 虚拟DOM

js运行速度快，大量操作dom就慢。

时长数据更新后悔重新渲染页面，没有改变数据的地方也重新渲染了DOM节点（页面的重绘和重排），造成资源浪费

利用内存中生成与dom对于的数据结构在内存中生成的结构为虚拟DOM

数据改变时，重绘和重排的代价就最小到了DOM操作


# vue的实例

var vm=new Vue()；

方法：


- el： 绑定的dom
- data： 使用的数据
- methods： 绑定的方法
- prop： 数据代理，data里的数据时被代理的，后面新添加的不会响应，改变后不会改变视图

自生的属性和方法-$开头

$el,$data...



# 声明式渲染和命令式渲染

声明式：一部一部来


	const arr=[1,2,3,4,5];
	/*=========声明式写法=========*/
	const newArr1=[];
	for(let i=0;i<arr.length;i++){
	    newArr1.push(arr[i]*2)
	}
	/*===========命令式写法===============*/
	const newArr2=arr.map((item)=>{
	    return item*2;
	});


# 指令


- v-bind   动态绑定属性，否则为死数据   :
- v-text   更新数据，会覆盖已有结构
- v-show   切换display
- v-else-if  判断是否渲染
- v-for   基于数据多次渲染元素
- v-pre 跳过元素和子元素的编译过程
- v-cloak 隐藏未编译前的模板数据， [v-cloak]{display:none}
- v-on  绑定监听事件  @
- v-html 解析数据中的html结构
- v-if  切换元素会被销毁重建
- v-model 表单空间元素上创建双向绑定数据
- v-once 只渲染一次  随后数据更新不会重新渲染

# 模板

## html模板

    - 文本 {{value}}，根据data里的数据双向绑定
    - 原生的html {{}}是文本，不会解析html，用 v-html=‘{{}}’绑定
    - 用v-bind绑定可以响应式
    - 里面可以进行一些简单的表达式，二元三元表达式


## templete模板

在实例对象里写，注意根节点只有1个，后面的会被忽略

必须要有个div，否者会报错

正确：

	<div class='box'>
	    <h1>hello world1111</h1>
	</div>


错误，span会被忽略：

	
	<div class='box'>
	    <h1>hello world1111</h1>
	</div>
	<span>131</span>



也可以直接写在script里

	<script type="x-template" id="ot"></script>



## render函数模板

	render(createElment){
	    //createElment(元素，{属性：值}，[子元素])
	    return createElment(
	       'ul',
	       [
	           createElment('li',1),
	           createElment('li',1),
	           createElment('li',2),
	       ]
	    );
	}

## 组件

原理：

- 注册组件   全局注册和局部注册
	
        Vue.conpoment('标签名'，{template：标签})  别用驼峰
    
        在实例的对象{conpoment：{}}中注册


- 传送数据

父——子  利用prop在标签上自定义属性上，然后子标签通过获取属性值来得到值。

     父 —— :OOXX=数据;
     子 ——  props：[OOXX]   ：vaule=数据


子——父  利用自定义事件，然后通过$.emit的方法来返回

     父 —— @fn='处理程序'  //value 
     子 —— @click=function(){this.$emit('fn',数据)}



## 组件里利用了闭包来让每个组件的值独立话


## 标签上的is=组件名，是防止当DOM包裹不符合W3C属性的时候报错




- css  
- js
- data


		<template>
		    <p>{{data}}</p>
		</template>
		
		<script>
		    module.exports{
		        data:function () {
		            return {
		                data:1,
		                name:123
		            }
		        }
		    }
		</script>
		<style scoped>
		    p{
		        background-color: #fff;
		    }
		</style>



## vue-cli


安装 -


name- 项目名称

vue-router  装不装

ESLint   语法检查 N




## vuex


- store(仓库) —— 一个容器，包含状态，存储是响应式的，不能直接改变
- state（状态）—— 单一状态树，一个对象
- getter：操作状态的逻辑，被多组件使用
- mutation（修改状态的唯一路基）  commit必须是同步的，有异步的时候store的值依旧为原来的值
- action（异步操作）



步骤：

### 安装  

		npm i vuex --save

### 定义容器   

新建一个store文件，在其中新建index.js

	import Vue from 'vue'
	import Vuex from 'vuex'
	Vue.use(Vuex);
	
	const store= new Vuex.Store({
	
	});
	
	export default store  //输入这个store

### 在main中使用并且配置在vue的实例中

		import store from './store/index'
		
		new Vue({
		  el: '#app',
		  store,
		  components
		});

### 使用状态码

		const store= new Vuex.Store({
		   state:{
		       count:10
		   }
		});

        //组件中使用

		num:this.$store.state.count

### 改变状态

使用mutations的方法，


		const store= new Vuex.Store({
		   state:{
		       count:100
		   },
		    mutations:{
		       updataCount(state){ //上面的state传过来的
		           state.count+=1;
		       }
		    }
		});

在插件中commit其中方法

    methods:{
        clickbtn(){
            this.$store.commit('updataCount');
        }
    }

因为data只能在本组件内改变，所以数据要用计算属性computed，否则数据不会改变


        computed:{
          m(){
              return this.$store.state.count;
          }
        },
		<span>{{m}}</span>

也可以不适用data直接使用上面的值：

	<span>{{this.$store.state.count}}</span>

在方法里直接commit即可


### 模块给仓库传值


	 this.$store.commit('mutations方法名',对象);


     mutations:{
       mutations方法名(state,payload){ //上面的state传过来的
           payload //传过来的对象
       }
    }


# 两个核心

vue


# 前言

过去曾经自学了vue 1，但公司项目一直没涉及到vue，不知不觉vue已经踏入了2.0。终于最近的项目使用了vue，特意写一遍学习笔记来记录一下。


# 双向绑定  


mvc和mvvm框架最大的区别在于：

- mvc中的controller更像是一个小公司的boss，各种数据都要自己获取，转化成想要的数据，再绑定到指定的view上。

- mvvm中的controller更像是一个大公司的boss。数据的转换，绑定什么的都由秘书view-model来完成，然后将整理好交给controller，只负责了下决定。

数据的双向绑定在其中扮演了关键的角色。


## 原理

vue使用了数据劫持，通过`Object.defineProperty()`这个方法来劫持各个属性的值赋予和改变。数据变动时，触发相应的回调。


一个简单的双向绑定案例：

	//html
	
	<input type="text">
	<span></span>

	//js
    const input=document.querySelector('input');
    const span=document.querySelector('span');
    const obj={
        'key':'默认'
    };
    Object.defineProperty(obj,'key',{
        set(newVal){
            span.value=newVal;
            span.innerHTML=newVal;
        }
    });
    document.addEventListener('keyup',(e)=>{
        obj.key=e.target.value;
    });


- 监听输入的input，input的值改变data的值
- data的值改变，触发defineProperty的set方法
- 在set方法内部改变对应的view值

## vue实现的目标

1 span内容与data数据绑定

2 input内容变化时，data数据同步变化   view-model

3 data数据变化时，span的内容同步变化   model-view

上面的简单案例实现了2和3，对于1的实现，vue使用了DocumentFragment


## DocumentFragment 文档片段

创建dom的时候常见使用的方法：

	createElement(tagname)  //<tagname></tagname>
	createTextNode(text)   //'text'
	createAttribute(name)  //name=''
	createComment(text)　  //<!--text-->
	createDocumentFragment()  //#document-fragment


因为reflow（layout）重排的问题，直接创造DOM去插入DOM树会很浪费性能，DocumentFragment文档片段就是针对这个问题产生的。






先把创建的元素放入DocumentFragment，然后再插入DOM，因为DocumentFragment不是真实DOM树的一部分，它的变化不会引起DOM树的重新渲染的操作(reflow) ，且不会导致性能等问题，这样就只会调用appendChild()，就只会一次reflow（layout）重排


- 把文档片段插入DOM时，只会把子节点插进去，本身是不会进入DOM。
- 把DOM的节点插入文档片段时，节点会从DOM。这个过程叫做劫持。

eg:

	//html
	<div class="box">
	    <ul>
	        <li>1</li>
	        <li>2</li>
	        <li>3</li>
	        <li>4</li>
	        <li>5</li>
	    </ul>
	</div>

	//js
    const  box=document.querySelector('.box');
    function nodeToFragment(node) {
        const fragment=document.createDocumentFragment();
        let child;
        while (child = node.firstChild){
            fragment.appendChild(child);
        }
        return fragment;
    }
    box.appendChild(nodeToFragment(box));




# 模板引擎的原理


	    for (key in obj) {
	        reg = new RegExp('{{' + key + '}}', 'ig');
	        t = (t || template).replace(reg, obj[key]);
	    }





# 生命周期


## 生命周期的钩子函数：

10个



- beforeCreate  组件实例刚被创建，组件属性计算前，如data
- created 	    组件实例创建完成，属性已绑定，但是DOM还未生成，$el不存在
- beforeMount   模板编译/挂载前
- mounted       模板编译/挂载后  不保证组件已在document中
- beforeUpdata  组件更新之前
- updated       组件更新之后
- activated     for  keep-alive,组件被激活时调用
- deactivated   for  keep-alive,组件被移除时调用
- beforeDestory 组件销毁前
- destoryed     组件销毁后



## 每个生命周期具体做了什么



	<!DOCTYPE html>
	<html>
	<head>
	    <title></title>
	    <script type="text/javascript" src="https://cdn.jsdelivr.net/vue/2.1.3/vue.js"></script>
	</head>
	<body>
	
	<div id="app">
	     <p>{{ message }}</p>
	</div>
	
	<script type="text/javascript">
	    
	  var app = new Vue({
	      el: '#app',
	      data: {
	          message : "xuxiao is boy" 
	      },
	       beforeCreate: function () {
	                console.group('beforeCreate 创建前状态===============》');
	               console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
	               console.log("%c%s", "color:red","data   : " + this.$data); //undefined 
	               console.log("%c%s", "color:red","message: " + this.message)  
	        },
	        created: function () {
	            console.group('created 创建完毕状态===============》');
	            console.log("%c%s", "color:red","el     : " + this.$el); //undefined
	               console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化 
	               console.log("%c%s", "color:red","message: " + this.message); //已被初始化
	        },
	        beforeMount: function () {
	            console.group('beforeMount 挂载前状态===============》');
	            console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
	            console.log(this.$el);
	               console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化  
	               console.log("%c%s", "color:red","message: " + this.message); //已被初始化  
	        },
	        mounted: function () {
	            console.group('mounted 挂载结束状态===============》');
	            console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
	            console.log(this.$el);    
	               console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
	               console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
	        },
	        beforeUpdate: function () {
	            console.group('beforeUpdate 更新前状态===============》');
	            console.log("%c%s", "color:red","el     : " + this.$el);
	            console.log(this.$el);   
	               console.log("%c%s", "color:red","data   : " + this.$data); 
	               console.log("%c%s", "color:red","message: " + this.message); 
	        },
	        updated: function () {
	            console.group('updated 更新完成状态===============》');
	            console.log("%c%s", "color:red","el     : " + this.$el);
	            console.log(this.$el); 
	               console.log("%c%s", "color:red","data   : " + this.$data); 
	               console.log("%c%s", "color:red","message: " + this.message); 
	        },
	        beforeDestroy: function () {
	            console.group('beforeDestroy 销毁前状态===============》');
	            console.log("%c%s", "color:red","el     : " + this.$el);
	            console.log(this.$el);    
	               console.log("%c%s", "color:red","data   : " + this.$data); 
	               console.log("%c%s", "color:red","message: " + this.message); 
	        },
	        destroyed: function () {
	            console.group('destroyed 销毁完成状态===============》');
	            console.log("%c%s", "color:red","el     : " + this.$el);
	            console.log(this.$el);  
	               console.log("%c%s", "color:red","data   : " + this.$data); 
	               console.log("%c%s", "color:red","message: " + this.message)
	        }
	    })
	</script>
	</body>
	</html>




- beforecreated：el 和 data 并未初始化 
- created:完成了 data 数据的初始化，el没有
- beforeMount：完成了 el 和 data 初始化，生成了Dragment但是没绑定数据 
- mounted ：完成挂载,生成了dragment同时也完成了数据绑定
- beforeUpdate：data里的数据改变的时候重复啊
- detory：在对应的实例`app.$destroy();`执行
  



# 具体作用


	beforecreate : 在这加个loading事件 
	created ：在这结束loading，还做一些初始化，实现函数自执行 
	mounted ： 取消loading，在这发起后端请求，拿回数据，配合路由钩子做一些事情
	beforeDestory： 你确认删除XX吗？ 
	destoryed ：当前组件已被删除，清空相关内容
