---
title: art-template基础
date: 2017-05-02 13:49:18
tags: js
---

# 前言 

这是一个前端字符模板的使用方案。不过还是推荐直接使用vue.js当字符模板


<!--more--><!--more-->



# 基础用法

## 输出

	{{value}}
	{{data.key}}
	{{data['key']}}
	{{a ? b : c}}
	{{a || b}}
	{{a + b}}


## 过滤器

	因为 imports 定义的全局变量的优先级会比普通模板变量高，所以建议命名使用 $ 前缀。
	
	{{date | $timestamp | $dateFormat 'yyyy-MM-dd hh:mm:ss'}}
	

	{{value | filter}} 过滤器语法类似管道操作符，它的上一个输出作为下一个输入。







## 引用文件


	<script src="dist/template.js"></script>

## artTenmplate部分


	<script id="idA" type="text/html">
	    {{if people}}
	
	
	    <ul>
	        <li>{{people.name}}</li>
	        <li>{{people.age}}</li>
	        <li>{{people.sex}}</li>
	        <li>{{add}}</li>
	        <li>{{fuck.data}}</li>
	    </ul>
	
	    {{/if}}
	</script>


## js部分

    var data={
        people:{
            name:"tom",
            age:18,
            sex:"boy"
        },
        add:"233",
        fuck:{
            data:666
        }
    };
    var html = template('idA', data);
    //console.log(html);
    $('.box').html(html);


# 语法

## 条件判断

	{{if 变量1}}        //变量1为，template中的data内的一个key值
	{{变量2}}          //代表变量，值等于js中的变量2的值
	{{/if}}


当data为json的情况：

    var data = {
     author: '宫崎骏',
     isAdmin: true,
     list: ['千与千寻', '哈尔的移动城堡', '幽灵公主', '风之谷', '龙猫']
     };
     var html = template('id1', data);
     $('.box').html(html);

	<script id="id1" type="text/html">
	    {{if isAdmin}}
	    <ul>
	        <li><h1>{{author}}</h1></li>
	        {{each list}}
	        <li>{{$index}}.{{$value}}</li>
	        {{/each}}
	    </ul>
	    {{/if}}
	</script>


当data为数组的情况：

	[{id: "12", nickname: "张三", content: "测试内容12"}]
	直接在var html = template('idA', data[0]); {{if id}}即可

	//当为多个元素的情况：

    var data=[
        {
            name:"tom",
            age:18,
            sex:"boy"
        },{
            name:"jack",
            age:19,
            sex:"girl"
        }
    ];


	//利用循环和拼接即可
    var html="";
    for(var i=0; i<data.length; i++){
        html+= template('idA', data[i]);
    }
    console.log(html);
    $('.box').html(html);


	//做成一个新的数据在传入
    var data=[
        {
            name:"tom",
            age:18,
            sex:"boy"
        },{
            name:"jack",
            age:19,
            sex:"girl"
        }
    ];
    var data1={data:data};
    var html= template('idA', data1);
    $('.box').html(html);
	<script id="idA" type="text/html">
	    {{if data}}
	
	
	    <ul>
	        <li>{{data[0].name}}</li>
	        <li>{{data[1].name}}</li>
	    </ul>
	
	    {{/if}}
	</script>



情况2：如果标签里面有“”需要带入数据

	<img src="{{value1.img}}" alt="">    不用连字符


情况3：变量为伪数组，不要加[i]用自己即可

	var data={
	    name:"tom",
	    albums:["http://juheimg.oss-cn-hangzhou.aliyuncs.com/cookbook/t/0/9_653116.jpg"]
	};
	var html=template("id",data);
	$(".box").html(html);
	<script id="id" type="text/html">
	    {{if albums}}
	    {{each albums as value index}}
	    <li><img src="{{value}}" alt=""></li>
	    {{/each}}
	    {{/if}}
	</script>


## 循环


	{{each target}}
	    {{$index}} {{$value}}
	{{/each}}


	{{each albums as value index}}
	{{/each}}

# template.compile(str)

因为一直在html中增加`script`标签有时候看着比较麻烦，所以可以尝试全写在js代码中，这样维护什么的比较方便。

        var data3={
            text:"hello world"
        };
        var str="";
        str+="<h1>{{text}}</h1>";
        var render=template.compile(str);
        var html=render(data);
        $('.box').html(html);

# template.helper

相当于自己添加函数功能，对传入的数据进行处理，直接返回想要达到的结果。

	template.helper('add',add);

    function add(a,b){
        return a+b;
    }

    var data = {
        a: 3,
        b: 4
    };

    var html = template('id', data);
	$('.box').html(html);


	<script id="id" type="text/html">
        {{add(a,b)}}
        {{value.score | add:1}}
    {{value.class | actor:value.direct,value.starring }}
	</script>


推荐下一个方式，value.score为第一个参数，add为方法：后面为第二个参数

eg:

    {{value.score}}分

    template.helper('copyright_logo', function(flag) {  //判断是不是有误版权商标
        var html="";
        (+flag==1)&&(html+='<lable class="huashu">华数</lable>');
        return html;
    });