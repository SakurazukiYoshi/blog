---
title: 常用js
date: 2017-05-02 09:03:58
tags: js
categories: js
---


# 前言

这是在工作上js方面遇见的问题和解决方案




<!--more--><!--more-->



# webstorm的快捷键


ctrl+a  全选

# 添加数组的方式

向尾部添加：

1. arr[arr.length] = 6; // 平均42 345 449 ops/sec
2. arr.push(6); // 慢34.66%
3. arr2 = arr.concat([6]); // 慢85.79%

向头部添加：


1. [0].concat(arr); // 平均4 972 622 ops/sec
2. arr.unshift(0); // 慢64.70%


向数组中间添加：


var items = ['one', 'two', 'three', 'four'];
items.splice(items.length / 2, 0, 'hello');




## 添加不支持的文件为已经支持的格式

	微信小程序官方说明需要在微信开发者工具中开发运行，但这个工具着实不咋地。
	
	我是使用webstrom编辑，然后在微信开发者工具中热加载查看效果，因为webstrom默认并不支持*.wxml，添加使用xml（各人随意，我使用html格式显示不好看）格式编写。
	
	　　File -> Settings -> Editor -> File Types
	
	在Recognized File Types 中找到你想使用的格式，然后在Registered Patterns 中添加 *.xml。




#  return false的应用


return false的实际作用：

	•event.preventDefault();   //阻止元素的默认行为
	•event.stopPropagation();  //阻止事件冒泡
	•停止回调函数执行并立即返回。

对于a标签只是用于点击而不需要执行跳转的情况可以使用：

	<a href="#m1"  onclick="return false">

这样网页上面不会出现点击a标签后的锚点。当然这样多于用了很多功能，实际上起作用的是：

	$("a").click(function(event){
	  event.preventDefault();  
	});

改方法同样可以用于按钮按了以后对表单的提交


# 兄弟节点

	
	$('#id').siblings() 当前元素所有的兄弟节点
	$('#id').prev() 当前元素前一个兄弟节点
	$('#id').prevaAll() 当前元素之前所有的兄弟节点
	$('#id').next() 当前元素之后第一个兄弟节点
	$('#id').nextAll() 当前元素之后所有的兄弟节点


# 常用的正则表达式

	
	手机号码：   /^1[3|4|5|7|8][0-9]{9}$/


	/g意思就是：表示替换将针对行中每个匹配的串进行，否则则只替换行中第一个匹配串。

	/i意思就是 case insensitive,区分大小写小字。

	/d意思是digital.是一个数字如：/d就相当于[0-9]




	/^[0-9]\d*([.][0-9]{1,2})?$/;保留两位小数


	{1,2}最少1个做多两个


  	 身份证：/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/

	// 身份证号码为15位或者18位，15位时全为数字，18位前17位为数字，最后一位是校验位，可能为数字或字符X


#  点击div以外的元素触发


	$(document).click(function(){//事件1
	    $("#bar").hide();   
	});

	$("#bar").click(function(event){//事件2
	    event.stopPropagation();   
	});


这里需要注意，点击document执行事件1，而事件2的目的是不执行事件1



#  没有的元素才能出发的事件

一些插件用增删class来触发事件，例如jquery.boxy关闭弹窗需要点击有.close的元素。然而很多情况页面设计上面没有.close这个元素，所以我们就需要在将其加在页面中隐藏起来，在其他事件触发的同时，触发隐藏元素的点击事件即可。

    $("body").click(function () {
        $("#closeImg").click();   //取消
    });

点击body出发closeImg元素







# 对string和array进行删除操作

推荐使用slice(a,b);  a开始，删到b    从0开始，只有1个值的时候默认为第二个值
当为-的时候用数组的长度来-即可



	var colors = ["red", "green", "blue", "yellow", "purple"];
	var colors2 = colors.slice(1);
	var colors3 = colors.slice(1,4);
	alert(colors2); //green,blue,yellow,purple
	alert(colors3); //green,blue,yellow



#  刷新当前的页面

window.location.reload()刷新当前页面.   //因为安卓不支持

解决方案：

	window.location.href = location.href+'?time='+((new Date()).getTime());


原因：
href是location对象的一个属性，reload()则是location对象的方法

所以对于href，可以为该属性设置新的 URL，使浏览器读取并显示新的 URL 的内容。

对于reload()则是重新加载当前文档，如果该方法没有参数或者参数为 false，会用 HTTP 头 If-Modified-Since 来检测服务器上的文档是否已改变。如果文档已改变，reload() 会再次下载该文档。如果文档未改变，则该方法将从缓存中装载文档。如果把该方法的参数设置为 true，那么无论文档的最后修改日期是什么，它都会避开缓存，从服务器上重新下载该文档。

对于安卓手机微信中浏览器，reload是从缓存中装载文档，所以是失效的；



其他：

	1，history.go(0) 
	2，location.reload() 
	3，location=location 
	4，location.assign(location) 
	5，document.execCommand('Refresh') 
	6，window.navigate(location) 
	7，location.replace(location) 
	8，document.URL=location.href

	自动刷新：
	<meta http-equiv="refresh" content="20">
	
	function myrefresh()
	{
	   window.location.reload();
	}
	setTimeout('myrefresh()',1000); //指定1秒刷新一次






# 阻止默认事件


	1.event.stopPropagation()  阻止事件的冒泡方法
	
	2.event.preventDefault()阻止默认事件的方法
	
	3.return false  包含以上两种

在大多数情况下,return false,可以防止默认的事件行为.例如,默认情况下点击一个a元素,页面会跳转到该元素href属性指定的页面.

不过return false实际做的事情是

	event.preventDefault();
	
	event.stopPropagation();
	
	停止回调函数执行并立即返回。

所以在使用return false前还是要考虑一下阻止冒泡了会不会有影响。



兼容性写法：


	e.stopPropagation ? e.stopPropagation() : (e.cancelBubble=true); //阻止冒泡




# 刷新页面以后，需要之前数据的处理


## 方法1

使用html5的Storage，ie8一下的浏览器不支持。

localStorage - 一直存在

sessionStorage - 在浏览器关闭前一直存在


    if(sessionStorage.item){   //临时缓存存在的时候，显示的是之前点击的内容
        $showButton.html(sessionStorage.item);
    }else {  //当临时缓存不在的时候，下拉列表中的显示的内容是想象中的第一个
        $showButton.html($showList.find(".listItem").eq(0).html());
    }

要注意Storage中只能存储字符串，所以要结合JSON.parse(str)和JSON.stringify(obj);使用




## 方法2

利用点击后url变化的原理在url中添加参数，然后从刷新的页面中读取参数来获取之前的数据，注意其中涉及到中的时候要使用url编码和解码
 
利用search  url?

    if(window.location.search){
        $("select").val(decodeURIComponent(window.location.search).substr(1));
    }
    $(function(){
        $("#a").click(function(){
            window.location.search="?"+$("select").val();  //给url+#下拉列表选中的值
            $(this).attr("href",window.location);  //将url添加到a标签的href上
            return false;
        });
    })


利用hash url#

    if(window.location.hash){
        $("select").val(window.location.hash.substr(1));
    }
    $(function(){
        $("#a").click(function(){
            console.log(1);
            window.location.hash=$("select").val();  //给url+#下拉列表选中的值
            location.reload(window.location.href);
            return false;
        });
    })




# 自动获取焦点

方式1：

        给input  添加autofocus属性

方式2：

         $(input).focus()      //注意要在input生成以后才用不然无效


#  关闭浏览器预测对字段的输入


autocomplete="off"


# 解决缓存问题  

最开始工作是手机端的页面，每次修改后，测试的手机因为缓存显示以前的css，很讨厌

在css和js文件的最后加上?随便         

	eg：         xxx.js?v=0.101


# ajax上传的数据处理


json字符串转json对象：jQuery.parseJSON(jsonStr);

json对象转json字符串：JSON.stringify(jsonObj);   


对于数组同样也使用，将json转化为字符串，将json字符串转化为json


查看数据的类型：toString.call()

eg:
post的文件：

        data: {
            shopName:messages.shopName,

            roomNumber:messages.roomNumber,

            shopAddress:messages.shopAddress,

            peopleName:messages.peopleName,

            telephone:messages.telephone,

            industry:JSON.stringify(messages.industry),   
			//数组要这样转化，将其转化为json字符串

            images:JSON.stringify(messages.images)
        },


# 	无法被动侦听事件preventDefault

	Unable to preventDefault inside passive event listener due to target being treated as passive.

解决办法：	
	
	* { touch-action: none; } 



# 定时清除localStorage的办法

localStorage一直放在本地也不是办法，试着思考了一种可以定时清除的办法，不清楚对不对

    setInterval(function(){
        var date=new Date();
        var s=parseInt(date.getTime()/1000);
        if(!localStorage.time){
            localStorage.time=s;
        }else {
            if (s>=parseInt(localStorage.time)+10){ //每10秒清除一次
                localStorage.clear();
                alert("清除了");
            }
        }
    },1000);



# 获取指定时间的js


	var date=new Date("2017.04.12 00:00:01");



# url的编码和解码


decodeURIComponent():统统会被编码


	编码：Javascript:encodeURIComponent("春节");
	
	解码：Javascript:decodeURIComponent("%E6%98%A5%E8%8A%82")


encodeURI():返回编码为有效的统一资源标识符 (URI) 的字符串。

不会被此方法编码的字符：! @ # $ & * ( ) = : / ; ? + '


	编码：Javascript:encodeURI("春节");
	
	解码：Javascript:decodeURI("%E6%98%A5%E8%8A%82");



# 上一页的地址


document.referrer


#  去掉首尾的空格


	var str = "       hl   ";
	var patt1 = /(^\s*)|(\s*$)/g ;
	var str1 = str.replace(patt1,"");
	console.log(str1)

一句表达式：

	$("#input").val().replace(/(^\s*)|(\s*$)/g,"")



# 监听是否点击返回上一页和下一页按钮,并且不返回


    $(function(){
        pushHistory();
        window.addEventListener("popstate", function(e) {
            alert("我监听到了浏览器的返回按钮事件啦");
        }, false);
        function pushHistory() {
            var state = {
                title: "title",
                url: "#"
            };
            window.history.pushState(state, "title", "#");
        }
    });


#   阻止返回上一页


    window.history.pushState(sessionStorage.item, "", window.location.href);
    $(window).on("popstate", function() {   //点击了返回上一页
        console.log(1);
        window.history.pushState(sessionStorage.item, "", window.location.href);
    });



## popstate事件,用于控制浏览器历史记录的

当活动历史记录条目更改时，将触发popstate事件,调用history.pushState()或history.replaceState()不会触发popstate事件,只有在做出浏览器动作时，才会触发该事件:如用户点击浏览器的回退按钮或者代码中调用history.back()


过去用于操作历史记录的window.history对象

属性：  
	
	length：返回浏览器历史列表中的 URL 数量

	state：

方法：

	back()      返回上一页

	forward()   进入下一页

	go()    正数表示下几页，负数表示上几页

缺陷：这三种方法都是无法控制前进后要去哪里，同时histor.length维持原来的状态


html5扩展了一些方法：

	pushState()      //存储当前历史记录点

	replaceState()   //替换当前历史点

	popstate()      //监听当前历史点

eg：

	history.pushState(data,title,url);

data是给state的值,title为页面的标题，传空字符串即可，url是你想要去的链接


	history.replaceState("首页","",location.href+ "#news");

区别：replace不会改变history的length，push会改变



eg:

    $(function () {
        var i=0;
        $("a").click(function(e){
            e.preventDefault();
            window.history.pushState(i, "title", "#"+i);
            i++;
        });
        $(window).on("popstate", function() {   //点击了返回上一页
            console.log(1);
        });
    });



# 五星评价的半星评价

平时评价都是1个整星的评价，但是如果出现半星评价的要求就需要对星星进行相应的处理。

思路：将一个整星的元素分成左右两个元素。这就需要用到将图片可控制大小+显示部分的功能。暂时想到了两个思路来解决：

## 利用background来解决


    background: #fff url(star.png) 0 0 no-repeat;

可以将元素大小定死，利用position来调整图片显示的部分很方便。然而background一般是将图片的大小按原本大小展示的，对于图片大小不适合的要进行修改大小，这个时候就要利用css3新增的`background-size`属性

	background-size:80px 60px;

	background-size:10% 10%;

	background-size:cover ;    //平铺

	background-size:contain	 ; //让图片的高或者宽一边铺满


整体的思路是：


    .box{
        width: 12px;
        height: 24px;
        background: #fff url(star.png) 0 0 no-repeat;
        background-size: 24px
    }
    .box1{
        width: 12px;
        height: 24px;
        background: #fff url(star.png) -12px 0 no-repeat;
        background-size: 24px
    }

	<div class="box" ></div>
	<div class="box1" ></div>


当然这样的缺点是：因为`background-size`是css3增加的属性，兼容性有问题最低支持IE9,IE9以下的浏览器就不推荐这种办法。


## 利用img和div的配和来解决


img元素比起background属性，优点是可以轻松的控制图片的大小，缺点也很明显就是img会把整个图片都显示出来。如果只想要显示半张图片就得利用一下div的配合了。

思路是让父元素只有img元素宽度的一半，超出的隐藏。偏移的部分用margin来控制。

    ul li{
        width: 12px;
        height: 24px;
        float: left;
        overflow: hidden;
        list-style: none;
    }
    ul li img{
        width: 24px;
        height: 23px;
    }
    ul li .star-right{
        margin-left: -12px
    }

	<ul>
	    <li><img src="star.png"></li>
	    <li><img src="star.png" class="star-right" ></li>
	</ul>


#  iconfont的使用

图标文字比使用图片更方便，然而一次性引用整个font-awesome字体库还是有点浪费，所以iconfont.cn的只用需要的图标更好用。


svg文件的使用：   ie9支持内联使用

 	<embed src="star.svg" type="image/svg+xml"></embed>

	<img src="star.svg" ></img>   //静态图片使用


或者直接在用`<i>`图标来使用，兼容性良好，支持ie8+，color无效

 
将需要的图标文字放入购物车，点击下载代码，然后解压，注意不能把单独的css文件提取出来，必须保持解压的状态

	<link rel="stylesheet" type="text/css" href="./iconfont.css">


	<i class="iconfont icon-xxx"></i>


最原始的方式：支持ie6+，unicode引用,color有效


    <style>
        @font-face {
            font-family: 'iconfont';
            src: url('star/iconfont.eot');
            src: url('star/iconfont.eot?#iefix') format('embedded-opentype'),
            url('star/iconfont.woff') format('woff'),
            url('star/iconfont.ttf') format('truetype'),
            url('star/iconfont.svg#iconfont') format('svg');
        }
        .iconfont{
            font-family:"iconfont" !important;
            font-size:16px;font-style:normal;
            -webkit-font-smoothing: antialiased;
            -webkit-text-stroke-width: 0.2px;
            -moz-osx-font-smoothing: grayscale;
        }
    </style>


	<i class="iconfont">&#x33;</i>


# 禁止点击事件


方法1：

	如果是通过a标签来实现点击的话可以增加css


	.disabled { pointer-events: none; }

方式2：

	如何是button元素的话可以增加disable属性
	$('#button').attr('disabled',"true");添加disabled属性
	$('#button').removeAttr("disabled"); 移除disabled属性

方法3：


	$(this).click(function (event) {
		event.preventDefault();
	}


# 字符串的截取
	
	
	slice()	    开始位置	需要返回的子字符串最后一个字符后面的位置
	substr()	开始位置	需要返回的字符个数
	substring()	开始位置	需要返回的子字符串最后一个字符后面的位置

# 限制input的输入长度

	oninput="(value.length>4)&&(value=value.slice(0,4))"



# js日期格式化


/* 格式化时间方法 */

    function dateFormat(date, format) {      //输入对应的时间和需要的输出的格式代码 MM:dd 输入的就是月份：日
        date = new Date(date * 1000);   //输入的时间转换为毫秒
        var map = {
            "M": date.getMonth() + 1, //月份
            "d": date.getDate(), //日
            "h": date.getHours(), //小时
            "m": date.getMinutes(), //分
            "s": date.getSeconds(), //秒
            "q": Math.floor((date.getMonth() + 3) / 3), //季度
            "S": date.getMilliseconds() //毫秒
        };
        format = format.replace(/([yMdhmsqS])+/g, function (all, t) {    //format必须由yMDHMSQS组成
            var v = map[t];
            if (v !== undefined) {
                if (all.length > 1) {
                    v = '0' + v;
                    v = v.substr(v.length - 2);
                }
                return v;
            }else if (t === 'y') {
                return (date.getFullYear() + '').substr(4 - all.length);
            }
            return all;
        });
        return format;
    }

需要的内容，开始的时间日期的getTime()和需要用的时间的毫秒数，
两者相加算数data，然后在根据需要的输出的时间写出format即可


# parseInt(string, radix)

将字符串转化为整数，后面一个参数是用于表明转化的进制


# 转义字符"/"

	<script>
    	alert("</script>");    //报错
	</script>



	<script>
    	alert("<\/script>");
	</script>


# 过滤器filter()


在筛选中的元素中在找到对应的指定类型

和find的区别：

find是在子元素中寻找指定的类型

filter是在自身元素中寻找指定的类型

	
	<div class="css">
	    <p class="rain">测试1</p>
	</div>
	<div class="rain">
	    <p>测试2</p>
	</div>



    var $find =  $("div").find(".rain");
    console.log($find) ;
    var $filter = $("div").filter(".rain");
    console.log($filter);
	

结果：

	[p.rain, selector: Array[1]]
	[div.rain, selector: Array[1]]


# vue，angular和artTemplate的过滤器


过滤器是一个通过输入数据，能够及时对数据进行处理并返回一个数据结果的简单函数

	{{ "lower cap string" | uppercase }}   //结果：LOWER CAP STRING 
	{{ "TANK is GOOD" | lowercase }}     //结果：tank is good 


	{{ 1304375948024 | date:'medium'}}   //May 03, 2011 06:39:08 PM 
	{{ 1304375948024 | date }}             //结果：May 3, 2011 
	{{ 1304375948024 | date:"MM/dd/yyyy @ h:mma" }}   //结果：05/03/2011 @ 6:39AM 
	{{ 1304375948024 | date:"yyyy-MM-dd hh:mm:ss" }}  //结果：2011-05-03 06:39:08


自定义filter功能：

	
	angular.module('tanktest', []).filter('tankreplace', function() { 
	  return function(input) { 
	    return input.replace(/tank/, "=====") 
	  }; 
	}); 

	{{ "TANK is GOOD" | lowercase |tankreplace}}  //结果：===== is good 




    <li v-for="product in products|filterBy '水果' in 'category' |orderBy 'price' 1">


filterBy 过滤在 'category'中含有'水果' 关键字的列表，返回的列表就是只含有 '水果' 关键字的列表，而orderBy过滤器是根据价格做了一个升序，如果想要降序，只需要加一个小于0的参数；




Vue有两个内置的过滤器来过滤或者排序数据，分别是： filterBy 和 orderBy 。



# jquery中对动态生成的标签不会响应click事件

Jquery中对ajax动态生成的html标签不会响应click事件，需要使用.live来绑定。

解决办法：对于动态生成的元素obj用live来绑定事件

    obj.live("click",function(){
        alert(1);
    });

原本的事件：

    $("#spread").click(function(){
        $(".box").html(str+"<a href='javascript:' id='stop'>收起</a>");
    });

对于动态生成的a元素，怎么都绑定不了click事件。

尝试了用：

	<a href='javascript:fn()' id='stop'>收起</a>

可以在全局作用域的情况下，定义一个fn可以使用调用方法fn，然而这样要就传入的数据也必须是全局的变量，这样很不好，所以不推荐。网上查阅资料以后，明白是因为新创建的元素，on无法绑定事件，这个时候就需要使用专门用于这种情况的live来绑定。


W3C对于live的定义：live()方法适用于匹配选择器的当前及未来的元素。


jq1.7以后推荐使用on来代替其他事件绑定的方法，on代替live的写法：


	<div class="box" style="width: 200px;">
    	<a href="" id="stop"></a>
	</div>


注意要写全了才有效果，没有写全无效

    $(".box").on("click","#stop",function(){

    });


方法二  绑个动态id利用  onclick（fn（id））来解决

	function remove(id){
    	$("#film"+id).remove();
	}	


    <tr class="table-item" id="film1">
        <td >一条狗的使命</td>
        <td >电影</td>
        <td class="operate"><span class="orange" onclick="remove(1)">删除</span></td>
    </tr>



#	禁用浏览器的backspace默认回退事件


编辑文本的时候不小心按backspace会直接返回上一页，避免这个功能


	function doKey(e){
	    var ev = e || window.event;//获取event对象
	    var obj = ev.target || ev.srcElement;//获取触发事件的obj
	    var t = obj.type || obj.getAttribute('type');//获取事件源类型
	    if(ev.keyCode == 8 && t != "password" && t != "text" && t != "textarea"){
	        return false;
	    }
	}
	//禁止后退键 作用于Firefox、Opera
	document.onkeypress=doKey;
	//禁止后退键  作用于IE、Chrome
	document.onkeydown=doKey;


# $('',this)的意思


	$('.ui-selecter',this)选择的是现在选项中的child里面class是ui-selecter的
	意思跟jQuery(this).find(".ui-selecter");等同

# a&&b&&c

	a&&b是if(a){b}

	那么a&&b&&c&&dd

	是如果a为真那么返回b，如果b为真那么返回c一直连续下去

	1&&$("#text1").addClass('none')&&$("#text").html("123")&&$("#text").removeClass('text');

	这样就会执行下面的所有的操作


需要留意中间不能出现false否则后面的操作不会执行,

需要注意的是function没有返回值的时候，返回的是undefined，后续不会执行，若想要连续，需要返回一个不为false的内容


	(function(){console.log(1); return true})()&&(alert(1));


	(function(){console.log(1);})()&&(alert(1)); //不会执行


#	元素未显示设置width/height时，获取宽度为auto


	$(obj).width();
	用css("width")和offset().width都是auto


# this的指向问题
![](http://i.imgur.com/TKo6lJf.png)

	第一个this为window

	第二个this为e


个人思路：

	this指向的是调用这个函数的对象

	第一个this调用实际上是
	fn();   //后执行
	相当于window.fn();   
	所以调用的对象时window


	而第二个this是在
	fn().init()；  后执行
	所以想执行这个这个函数的对象是fn()；而fn返回的是e所以结果为e


# select选中的选择的index

	<select name="" class="btn">
	    <option value="11">1</option>
	    <option value="22">2</option>
	    <option value="33">3</option>
	    <option value="44">4</option>
	</select>


    $(".btn").click(function(){
        console.log($(this)[0].selectedIndex);
    })
	
	 //select 元素的自带属性   选中的选项的index，从0开始

获取选中文本：
  $("#ddlregtype").find("option:selected").text();


# checked判断是否选中

	$(this).find("input").is(':checked')

# 改变数据的保留小数位


	number.toFixed(1)   //保留小数1位

# 字符拼接的时候判断加不加元素


	str+=(value.flag == 1 ? '<span class="icon-star"></span>' : '')s;


# 手机端 点击号码直接呼叫号码



    <a href="tel:18980925161">12312312</a>


# 按后退键不刷新问题


因为后退键以后数据都是从缓存中加载的，部分其他动态数据无法正常显示
这个时候的做法可以是

-自动刷新页面，可以在缓存中增加一个条件来判断是否是返回页面，是就刷新

	(!+sessionStorage.count)&&(sessionStorage.count=1)&&(window.location.href = location.href+'?time='+((new Date()).getTime()));
	$(".goods_list").find("dl").click(function(){
		sessionStorage.count=0;
	});
	$("#orderGoods").click(function(){
		sessionStorage.count=0;
	});



-禁止用户缓存

	<meta http-equiv="Expires" CONTENT="0">
	<meta http-equiv="Cache-Control" CONTENT="no-cache">
	<meta http-equiv="Cache-Control" CONTENT="no-store">



浏览器自带的返回功能不会刷新页面，

	用 javascript 进行页面跳转既可达到返回的目的，又可刷新页面：window.location.href=返回页面 即可。

-最好的解决办法


	上一个页面

	(!sessionStorage.url)&&(sessionStorage.url=window.location.href);

	下一个页面
	
	window.history.pushState('', "title", "?123");
	$(window).on("popstate", function(e) {   //点击了返回上一页
		window.location.href=sessionStorage.url+'?time='+((new Date()).getTime());
		e.preventDefault();
	});



# 获取json的key值

    var json={
        name:"tom",
        age:18
    };
    function displayProp(obj){
        var keys=[];
        var values=[];
        for(var name in obj){
            keys.push(name);
            values.push(obj[name]);
        }
        console.log(keys);
        console.log(values);
    }
    displayProp(json);




# 判断IE浏览器的版本

	IE版本	支持的状态
	10及以下	document.all
	9及以下	document.all && !window.atob
	8及以下	document.all && !document.addEventListener
	7及以下	document.all && !document.querySelector
	6及以下	document.all && !window.XMLHttpRequest
	5.x	document.all && !document.compatMode


	下面的条件代码只会在IE7及一下浏览器中运行
	
	if (document.all && !document.querySelector) {
	alert('IE7 or lower');
	}
	下面这一个只会运行在IE8中，并且不支持IE7或者IE9：
	
	if (document.all && document.querySelector && !document.addEventListener) {
	alert('IE8');
	}
	下面的条件代码当浏览器为IE11+ 或者非IE时为真
	
	if (!document.all) {
	alert('IE11+ or not IE');
	}

	

	IE11或者非IE
	
	if (!document.all) {
	alert('IE11+ or not IE');
	}
	IE10
	
	if (document.all && document.addEventListener && window.atob) {
	alert('IE10');
	}
	IE9
	
	if (document.all && document.addEventListener && !window.atob) {
	alert('IE9');
	}
	IE8上面已经给出
	
	if (document.all && document.querySelector && !document.addEventListener) {
	alert('IE8');
	}
	IE7
	
	if (document.all && window.XMLHttpRequest && !document.querySelector) {
	alert('IE7');
	}
	IE6
	
	if (document.all && document.compatMode && !window.XMLHttpRequest) {
	alert('IE6');
	}
	检测IE版本
	
	var win = window;
	var doc = win.document;
	var input = doc.createElement ("input");
	
	var ie = (function (){
	//"!win.ActiveXObject" is evaluated to true in IE11
	if (win.ActiveXObject === undefined) return null;
	if (!win.XMLHttpRequest) return 6;
	if (!doc.querySelector) return 7;
	if (!doc.addEventListener) return 8;
	if (!win.atob) return 9;
	//"!doc.body.dataset" is faster but the body is null when the DOM is not
	//ready. Anyway, an input tag needs to be created to check if IE is being
	//emulated
	if (!input.dataset) return 10;
	return 11;
	})();

# laypage总页数只有1的时候不显示

自己加代码


    /*=========================总页数只有一页的情况=================================*/
    if(+total===1){
        var str="";
        str+='<div class="layui-box layui-laypage layui-laypage-default" id="layui-laypage-0">';
            str+='<a href="{:U('Home/Information/infoCenter')}?p=1" ><em class="layui-laypage-em"></em><em>1</em></> ';
        str+=' </div>';
        $("#page").html(str);
    }

# input框只能输入数字


	for(var i=0;i<$inputs.size();i++){
	    $inputs.eq(i).on("keydown",function(e){
	        var isNumber=0;
	        (e.keyCode>47&&e.keyCode<58)&&(isNumber=1); //上面的键盘数字
	        (e.keyCode>95&&e.keyCode<106)&&(isNumber=2);//小键盘数字
	        (e.keyCode==8 ||e.keyCode==9)&&(isNumber=3); //backspace
	        if(!isNumber){
	           return false;
	        }
	    });
	}



	方法一：禁止中文输入法 
	
	<input type="text"  style="ime-mode:disabled">
	 
	
	方法二：禁止黏贴，禁止拖拽，禁止中文输入法！
	 这种方法是最强的禁止 中文输入
	 <input type="text" onpaste="return false" ondragenter="return false" oncontextmenu="return false;" style="ime-mode:disabled"/>


# 阻止backspace返回上一次


        /*======================阻止backspace的默认行为=========================*/
        $( document ).keydown( function( event ) {
            if ( event.keyCode == 8 && !$("input").is(":focus")) {
                event.preventDefault();
            }
        } );

# 下载文件

	
	在做项目的时候经常会碰到上传下载,通常在上传完文件以后会把文件在项目中的相对路径存到数据库以便下载，如果想直接下载文件，不通过后台action，则可以直接把文件路径给a标签的href属性，例如：
	
	<a href="/uploadfolder/xxxx.txt">点击下载</a>
	这样用户在点击这个链接的时候，就会直接下载这个文件，但是这里有个问题，像txt，jpg这些浏览器支持直接打开的文件是不会执行下载的，而是会直接打开。这时候可以给a标签添加一个download属性，例如：
	
	<a href="/uploadfolder/xxxx.txt" download="文件名.txt">点击下载</a>
	download也可以不给值，这样就会使用默认的文件名。


# 判断多选框是否选中



	if ($('#checkbox-id').attr('checked')) {
	    // do something
	}


 	$('input[name="video_type"]:checked').val(); //获取多选选中的值




	require(['jquery'], function($) {   //判断是否登陆
	    $.ajax({
	        url: '/Home/Base/checkIsLogin ',
	        dataType: "json",
	        type: 'get',
	        success: function(data) {
	            //0  没有登陆  1登陆
	            if(data.status){
	                $("#login").addClass("none");
	                $("#register").addClass("none");
	            }
	        }
	    });
	});


# 清空测试线上的缓存


http://192.168.2.31/Home/Base/clear



# each的使用


  $(selector).each(function(index,element))


  $.each(arr1, function(i,val){      
      alert(i);   
      alert(val);
  });  

# 点击的自锁


			var double=true;
            if(double){
                double=false;
				obj.animate(json,function(){

				 double=true;
				})
            }


# 同步搜索

    var keywordLength = 0;
    //同步搜索
    $("#file_name").on("keyup", function() {
        var $value = $(this).val();
        var keyword = $value.replace(/(^\s*)|(\s*$)/g, ""); //除去空格
        var type = $("#film_type").val();
        if (keyword.length != keywordLength && keyword != "") { //当中文输入的时候才请求,关键字不为空
            post_ajax(keyword, type);
        }
        keywordLength = keyword.length;
        (keyword == "") && ($("#search-items").html(""));
    });


# layer弹出页面获取子页面元素的dom


    ,success: function(layero, index){
        var body = layer.getChildFrame('body', index);
        body.find("#close").click(function(){
             layer.close(index);
         })
    }


# 子页面让父页面刷新


    window.parent.document.location.href=url;


# btn点击置灰，不能点


    $this.removeClass('disabled').prop('disabled', false);


#  全选，完整的全选功能


        var numAll=$(".chk_id").size();
        $('body').on('click',"#selectAll", function(){
            var flag=$(this).is(":checked");
            $(".chk_id").each(function (i,val) {
                $(val).get(0).checked=flag;
            });
        });
        $("body").on("click",".chk_id",function () {
            var num=$(".chk_id").filter(":checked").size();
            if(num!=numAll){
                $("#selectAll").get(0).checked=false;
            }else {
                $("#selectAll").get(0).checked=true;
            }
        })


判断是否全选：

        $("input[name='']").length == $("input[name='[]']:checked").length






# input disable不能提交

提交的之前遍历所有的input，选中的增加一个对象的input type=“hidden” 将对应的name和val赋予即可


    $auto_del.find("input").each(function(i,val){
        if($(val).get(0).checked){
            var input="<input type='hidden' name='"+$(val).attr("name")+"' value='"+$(val).val()+"'>";
            $(val).parents(".auto-del").append(input)
        }
        $(val).eq(0).attr("disabled",true);
    });



# table设置了宽度百分比但是不生效的问题


要注意内容是中文还是英文，默认英文不换行，中文换行因此要设置中英文都强制换行


	中英文都换行：

      white-space:pre-wrap;
      word-break:break-all;


注意：设置中英文都换行以后空格也会被算在其中所以td中的内容尽量不要用空格


# 判断是否选中多选框


    if( $('input[name="chargeTypes[]"]:checked').length == 0 ){



# tr的上下顺序互换


    function downSort(k) {
        // 当前节点
        var _my = $(k).closest('tr');
        var _myval = _my.html();
        //当前节点的后一个兄弟节点
        var _next = $(k).closest('tr').next();
        var _nextval = _next.html();

        if (_nextval != null) {
            //互换
            _my.empty().html(_nextval);
            _next.empty().html(_myval);
        }
    }


    function upSort(k) {

        // 当前节点
        var _my = $(k).closest('tr');
        var _myval = _my.html();
        var index=$(k).closest('tr').data("index");
        if(index==1){
            return false;
        }
        //当前节点的前一个兄弟节点
        var _pre = $(k).closest('tr').prev();
        var _preval = _pre.html();
        //查找是否有th
        var th = _pre.find('th').html();
        if (th == null) {
            //互换
            _my.empty().html(_preval);
            _pre.empty().html(_myval);
        }
    }







    <empty name="infos">
        <include file="Public:nosearch" />
        <else />

    </empty>



# 判断是手机端还是pc端

	function browserRedirect() {
	    var sUserAgent = navigator.userAgent.toLowerCase();
	    var bIsIpad = sUserAgent.match(/ipad/i) == "ipad";
	    var bIsIphoneOs = sUserAgent.match(/iphone os/i) == "iphone os";
	    var bIsMidp = sUserAgent.match(/midp/i) == "midp";
	    var bIsUc7 = sUserAgent.match(/rv:1.2.3.4/i) == "rv:1.2.3.4";
	    var bIsUc = sUserAgent.match(/ucweb/i) == "ucweb";
	    var bIsAndroid = sUserAgent.match(/android/i) == "android";
	    var bIsCE = sUserAgent.match(/windows ce/i) == "windows ce";
	    var bIsWM = sUserAgent.match(/windows mobile/i) == "windows mobile";
	    if (bIsIpad || bIsIphoneOs || bIsMidp || bIsUc7 || bIsUc || bIsAndroid || bIsCE || bIsWM) {
	        console.log("phone");
	        $(".phone_icon").removeClass("none");
	    } else {
	        console.log("pc");
	        $(".phone_icon").addClass("none");
	    }
	}



# 滚动执行只加载一次


	(function() {
	    var finished = true;
	    function loadData() {
	        //xxxx
	        finished = true;
	    }
	    dom.onscroll = function() {
	        if(finished && this.scrollHeight - this.clientHeight == this.scrollTop) {
	            finished = false;
	            loadData();
	        }
	    }
	})();




# 注释报错

	js不要在大括号后面写注释


	错误：
	if ($("#worktype-id").val() == '2') {//工作类型
	   alert($("#worktype-id").val());
	 }
	
	js的注释不要紧挨着“{ }”写，会出错！！！



# 分享到朋友圈


 1.外部浏览器分享（UC的原理不明）

	  body下面加

    <div style="overflow: hidden;height: 0">
        <img src="/Public/home/images/share_logo01.jpg" alt="" style="">
    </div>


2.微信内部浏览器的分享

	js-sdk（后端的配置特别重要）

    wx.config({
        debug: false,
        appId: '{$sign["appId"]}',
        timestamp: {$sign["timestamp"]},
        nonceStr: '{$sign["nonceStr"]}',
        signature: '{$sign["signature"]}',
        jsApiList: ['onMenuShareTimeline','onMenuShareAppMessage','onMenuShareQQ','onMenuShareQZone','onMenuShareWeibo']
    });	.



# 鼠标点击事件


	 document.onkeydown=function(event){
	       var e = event || window.event || arguments.callee.caller.arguments[0];
	       if(e && e.keyCode==27){ // 按 Esc 
	           //要做的事情
	         }
	       if(e && e.keyCode==113){ // 按 F2 
	            //要做的事情
	          }
	         if(e && e.keyCode==13){ // enter 键
	             //要做的事情
	        }
	    };



# window下ajax返回204却走的error

	原因：是window上面用原生txt编辑器打开会导致编码有问题

	重新拉代码，下载的编辑器打开修改的，保证编辑器设置的文件编码是utf-8



#  js捕获鼠标右键菜单中的粘帖事件

	$("#input").bind('paste', function(e) { 
	var el = $(this); 
	setTimeout(function() { 
	var text = $(el).val(); 
	alert(text); 
	}, 100); 
	}); 


# 手机视频播放全屏

    //安卓
    if(myVideo.webkitRequestFullScreen){
        myVideo.webkitRequestFullScreen()
    }

	//ios默认全屏播放















        if(myVideo.webkitCancelFullScreen){
            myVideo.webkitCancelFullScreen();
        }


# 全屏播放时黑屏，需要重新加载一次


        myVideo.load();
        myVideo.play();

# 防止用户恶意输入

	function SaferHTML(templateData) {
	  let s = templateData[0];
	  for (let i = 1; i < arguments.length; i++) {
	    let arg = String(arguments[i]);
	
	    // 在替换中转义特殊字符
	    s += arg.replace(/&/g, "&amp;")
	            .replace(/</g, "&lt;")
	            .replace(/>/g, "&gt;");
	
	    // 不转义模板中的字符
	    s += templateData[i];
	  }
	  return s;
	}


# js代码中有script标签

	var sender = '<script>alert("abc")<\/script>'; // 恶意代码





# 数字格式化




	/*
	* 参数说明：
	* number：要格式化的数字
	* decimals：保留几位小数
	* dec_point：小数点符号
	* thousands_sep：千分位符号
	* */
	Vue.filter('number_format',window.number_format=function(number, decimals, dec_point, thousands_sep){
	
	    number = (number + '').replace(/[^0-9+-Ee.]/g, '');
	    var n = !isFinite(+number) ? 0 : +number,
	        prec = !isFinite(+decimals) ? 0 : Math.abs(decimals),
	        sep = (typeof thousands_sep === 'undefined') ? ',' : thousands_sep,
	        dec = (typeof dec_point === 'undefined') ? '.' : dec_point,
	        s = '',
	        toFixedFix = function (n, prec) {
	            var k = Math.pow(10, prec);
	            return '' + Math.ceil(n * k) / k;
	        };
	
	    s = (prec ? toFixedFix(n, prec) : '' + Math.round(n)).split('.');
	    var re = /(-?\d+)(\d{3})/;
	    while (re.test(s[0])) {
	        s[0] = s[0].replace(re, "$1" + sep + "$2");
	    }
	
	    if ((s[1] || '').length < prec) {
	        s[1] = s[1] || '';
	        s[1] += new Array(prec - s[1].length + 1).join('0');
	    }
	    return s.join(dec);
	});


# 删除数组中的某个元素


    let address_arr=[
        {
            area:'红光',
            building:18,
            unit:2,
            layer:6,
            number:1020
        },
        {
            area: '红光',
            building: 18,
            unit: 2,
            layer: 6,
            number: 120
        }
    ];
    console.log(address_arr.splice(0, 1));  //删除的元素
    console.log(address_arr);  //删除后的数组

splice和slice的区别：slice 不改变元素的内容，删除后还是原本的数组



# 生成一个随机0~3的随机数


     Math.round(Math.random() * 10 / 2.5)



# 循环延时

    for (let i = 0; i < 5; i++) {
    　　setTimeout(function () {
      　　　　console.log(i);
    　　}, i * 1000);
    }

   一般的延时会因为js的运行机制一口气现实 1 2 3 4 5没有时间间隔
，先运行fo，在运行setTimeout
   






# console.claen()自动循环


浏览器插件的问题



# 倒计时


    let t = this.validate_time;  //默认的倒计时时间
    countdown(t);
    function countdown(time) {
        if (time <= 0) {
            that.validate_message='获取验证码';
            that.validate_disabled=false;
            that.validate_time=t;
            return false;
        } else {
            time--;
            that.validate_message=`${time}秒后重新发送`;
            that.validate_disabled=true;
            that.validate_time=time;
            setTimeout(function () {
                countdown(time);
            }, 1000);
        }
    }



# 身份证的详细验证


const validateIdCard=function (idCard){
    //15位和18位身份证号码的正则表达式
    var regIdCard=/^(^[1-9]\d{7}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}$)|(^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])((\d{4})|\d{3}[Xx])$)$/;

    //如果通过该验证，说明身份证格式正确，但准确性还需计算
    if(regIdCard.test(idCard)){
        if(idCard.length==18){
            var idCardWi=new Array( 7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2 ); //将前17位加权因子保存在数组里
            var idCardY=new Array( 1, 0, 10, 9, 8, 7, 6, 5, 4, 3, 2 ); //这是除以11后，可能产生的11位余数、验证码，也保存成数组
            var idCardWiSum=0; //用来保存前17位各自乖以加权因子后的总和
            for(var i=0;i<17;i++){
                idCardWiSum+=idCard.substring(i,i+1)*idCardWi[i];
            }
            var idCardMod=idCardWiSum%11;//计算出校验码所在数组的位置
            var idCardLast=idCard.substring(17);//得到最后一位身份证号码
            //如果等于2，则说明校验码是10，身份证号码最后一位应该是X
            if(idCardMod==2){
                if(idCardLast=="X"||idCardLast=="x"){
                    console.log("恭喜通过验证啦！");
                }else{
                    console.log("身份证号码错误！");
                }
            }else{
                //用计算出的验证码与最后一位身份证号码匹配，如果一致，说明通过，否则是无效的身份证号码
                if(idCardLast==idCardY[idCardMod]){
                    console.log("恭喜通过验证啦！");
                }else{
                    console.log("身份证号码错误！");
                }
            }
        }
    }else{
        console.log("身份证格式不正确!");
    }
}



# 获取上传图片的宽高，并且浏览

以前任务上传图片很难，但经过实际做过以后发现其实也没有想象中那么难
特别是，h5以后的新api对获取图片信息有质的飞升，在此特意记录。


    function imgUpload() {
        var file = document.getElementById('img_upload');
        createReader(file.files[0], function (w, h) { alert(w + ' '+ h)});
    }
    createReader = function(file, whenReady) {
            var reader = new FileReader;
            reader.onload = function (evt) {
                var image = new Image();
                image.onload = function () {
                    var width = this.width;
                    var height = this.height;
                    if (whenReady) whenReady(width, height);
                };
                image.src = evt.target.result;
            };
            reader.readAsDataURL(file);
        }

 
FileReader兼容性IE11

window.URL.createObjectURL兼容性IE11


<input type="file"> 元素的files属性要IE10才支持


IE9不支持客户端预览。使用上传插件，上传到服务端，返回图片地址，从而实现预览。


总结：


IE 10以上 

    <input type="file"> +file.files[0]+window.URL.createObjectURL+FileReader


IE  9一下

    滤镜

    file.select();
    var reallocalpath = document.selection.createRange().text;


	var url = document.getElementById(sourceId).value;

    // 非IE6版本的IE由于安全问题直接设置img的src无法显示本地图片，但是可以通过滤镜来实现
    pic.style.filter = "progid:DXImageTransform.Microsoft.AlphaImageLoader(sizingMethod='image',src=\"" + reallocalpath + "\")";
    // 设置img的src为base64编码的透明图片 取消显示浏览器默认图片
    pic.src = 'data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==';



兼容性写法：


	<form enctype="multipart/form-data" name="form1">
	    <input id="f" type="file" name="f" onchange="change()" />
	    <div class="upload">上传图片</div>
	    <p>预览:</p>
	    <p>
	        <img id="preview" alt="" name="pic" />
	    </p>
	</form>

    function change() {
        var pic = document.getElementById("preview"),
            file = document.getElementById("f");

        var ext=file.value.substring(file.value.lastIndexOf(".")+1).toLowerCase();

        // gif在IE浏览器暂时无法显示
        if(ext!='png'&&ext!='jpg'&&ext!='jpeg'){
            alert("图片的格式必须为png或者jpg或者jpeg格式！");
            return;
        }
        var isIE = navigator.userAgent.match(/MSIE/)!= null,    //判断是不是IE
            isIE6 = navigator.userAgent.match(/MSIE 6.0/)!= null;
        if(isIE) {
            file.select();
            var reallocalpath = document.selection.createRange().text;
            console.log(reallocalpath);

            // IE6浏览器设置img的src为本地路径可以直接显示图片
            if (isIE6) {
                pic.src = reallocalpath;
            }else {
                // 非IE6版本的IE由于安全问题直接设置img的src无法显示本地图片，但是可以通过滤镜来实现
                pic.style.filter = "progid:DXImageTransform.Microsoft.AlphaImageLoader(sizingMethod='image',src=\"" + reallocalpath + "\")";
                // 设置img的src为base64编码的透明图片 取消显示浏览器默认图片
                pic.src = 'data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==';
            }
        }else {
            html5Reader(file);
        }
    }

    function html5Reader(file){
        var file = file.files[0];
        var reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = function(e){
            var pic = document.getElementById("preview");
            pic.src=this.result;
        }
    }



对于IE9一下的上传，可以利用iframe 隐藏来提交




# 框架设置默认值


	function fileUpload(options) {
	    var opts = options || {};
	    var func = function() {};
	    this.fileInput = opts.fileInput || null;
	    this.url = opts.url || '';
	    this.fileList = [];
	    this.onFilter = opts.onFilter || function(f) {return f;};        //选择文件组的过滤方法
	    this.onSelect = opts.onSelect || func;            //文件选择后
	    this.onProgress = opts.onProgress || func;        //文件上传进度
	    this.onSuccess = opts.onSuccess || func;        //文件上传成功时
	    this.onFailure = opts.onFailure || func;        //文件上传失败时;
	    this.onComplete = opts.onComplete || func;        //文件全部上传完毕时
	    this.init();
	}



# 简单的深拷贝


	let b=JSON.parse(JSON.stringify(arr));

可以拷贝json和arr  不能拷贝function



# 判断是否是构造函数

好处是构造函数才能new   对象

function fn（）{

   this. instanceof fn


}


#  import和module.exports 混用报错

在听取周哥的意见，将所有ajax提出的时候遇到的问题，

项目是Vue+webpack
	
	Uncaught TypeError: Cannot assign to read only property 'exports' of object '#<Object>'

查了一下是因为import和module.exports 混用报错


	import {normalTime} from './timeFormat';
	
	module.exports={
	　　normalTime
	};


webpack 2中不允许混用import和module.exports,

解决办法：

	import {normalTime} from './timeFormat';
	
	export default normalTime;


# vue的@+路径

vuecli中引用模块的时候总是报错

@ 等价于 /src 这个目录，免得你写麻烦又易错的相对路径

所以推荐文件都放在/src目录下，这样引用比较轻松简单



# 将json转成url形式


推荐使用qs  



    import qs from 'qs'
    let a={
        a:1,
        b:2,
        c:3
    }
    console.log(qs.stringify(a));
	a=1&b=2&c=3



自定义的方法：


	/**
	 * param 将要转为URL参数字符串的对象
	 * key URL参数字符串的前缀
	 * encode true/false 是否进行URL编码,默认为true
	 *
	 * return URL参数字符串
	 */
	const urlEncode = function (param, key, encode) {
	    if(param==null) return '';
	    let paramStr = '';
	    let t = typeof (param);
	    if (t == 'string' || t == 'number' || t == 'boolean') {
	        paramStr += '&' + key + '=' + ((encode==null||encode) ? encodeURIComponent(param) : param);
	    } else {
	        for (let i in param) {
	            let k = key == null ? i : key + (param instanceof Array ? '[' + i + ']' : '.' + i);
	            paramStr += urlEncode(param[i], k, encode);
	        }
	    }
	    return paramStr;
	};
	/*
	
	
	var obj={name:'tom','class':{className:'class1'},classMates:[{name:'lily'}]};
	console.log(urlEncode(obj));
	//output: &name=tom&class.className=class1&classMates[0].name=lily
	console.log(urlEncode(obj,'stu'));
	//output: &stu.name=tom&stu.class.className=class1&stu.classMates[0].name=lily
	
	*/





# 类型转换


基本的数据类型

基本数据类型 

值类型：
number string boolean null undefined 

symbol

引用类型
 object function array regExp等



检验数据类型的方法：


typeof 适合基本类型和function类型的检测，无法判断null与object


instanceof 适合自定义对象，也可以用来检测原生对象，在不同的iframe 和 window间检测时失效，还需要注意Object.create(null)对象的问题

原理：判断变量的原型链上是否有构造函数的prototype属性


		// 判断person是否是object类型对象
		person instanceof Object


{}.toString 适合内置对象和基元类型，遇到null和undefined失效（IE678返回[object Obejct]）

Object.prototype.toString();




其他转str

值类型：

    console.log(String(1) );   // '1'
    console.log(String('123'));  //'123'
    console.log(String(null));     //'null'
    console.log(String(true));    //'true'
    console.log(String(undefined));  //'undefined'

引用类型：


对象到字符串

- 如果对象有 toString() 方法，就调用 toString() 方法。如果该方法返回原始值，就讲这个值转化为字符串。

- 如果对象没有 toString() 方法或者 该方法返回的不是原始值，就会调用该对象的 valueOf() 方法。如果存在就调用这个方法，如果返回值是原始值，就转化为字符串。

- 否则就报错



其他转Number 


基本（值）类型：


    console.log(Number(1) );     //1
    console.log(Number('123'));   //123
    console.log(Number('123abc'));   //NAN
    console.log(Number(null));    //0
    console.log(Number(true));    //1
    console.log(Number(undefined));   //NAN


加减法：
	
	console.log(1+'adc');   //1abc
	console.log(parseInt('1231addfs'));   //1231
	console.log(parseInt('a1231addfs'));   //NAN
	console.log(parseInt('addfs12321'));   //NAN


对象到数字：


区别在于转换为数字先判断 valueOf 方法，再判断 toString 方法。


基本转换为布尔：

	undefined
	null
	-0
	+0
	NaN
	‘’（空字符串）

其余的全是true


对象转为布尔

所有对象的布尔值都是true，甚至连false对应的布尔对象也是true。
请注意，空对象{}和空数组[]也会被转成true。



	var obj = new Boolean(false);
	console.log(obj && true);//true
	console.log(true && obj);//false


&& 表达式从第一个开始,遇到值为false的表达式,则返回表达式本身,否则返回最后一个表达式

|| 和 ! 逻辑运算符原理类似




## valueOf与toString方法研究:


所有JS数据类型都拥有这两个方法，null除外

valueOf() 方法，它的作用是：

如果对象存在任意原始值，它就默认将对象转换为表示它的原始值，如果对象是复合值，而且大多数对象无法真正表示为一个原始值，因此默认的valueOf( )方法简单地返回对象本身，而不是返回一个原始值。


toString(): 将该对象的原始值以字符串形式返回。

默认 [object class]
  class是对象的名称：Object String Number Function等

Array  将Aarry的元素转换为字符串，结果由字符串由逗号连起来

Boolean  true-‘true’  false-‘false’

Number   返回数字的文字表示





数值运算里，会优先调用valueOf()，在字符串运算里，会优先调用toString()

![](https://segmentfault.com/img/bVXTUy?w=668&h=384)



## 常见的算法：


![](https://segmentfault.com/img/bVXTUB?w=643&h=510)



		toString():
		
		
		console.log(({}.toString()));;       //=>  "[object Object]"
		console.log([1, 2].toString());;      //=>  "1,2"
		console.log(true.toString());;       //=>  "true"
		console.log(new Date(1970, 0, 1).toString());;  //=>  "Thu Jan 01 1970 00:00:00 GMT+0800 (CST)"
		
		console.log(Error("一个错误信息").toString());;    //=>  "Error: 一个错误信息"
		
		console.log((function (x) {
		    return x
		}).toString());;   //=>  "function (x){return x}"
		
		console.log(/\d/.toString());;    //=>  "/\\d/"  或者 "/\d/" 浏览器不同返回也可能会不同



		[1,2].valueOf();  //=>  [1,2]  
		
		(function (){}).valueOf();   //=>  function (){}
		
		/\d/.valueOf();    //=>  /\d/  
		
		new Date().valueOf();   //=>  1502941383029



	1. []+[] // ""


 首先运算符是 + 运算符而且很明显是二元运算符，并且有对象，所以选择最后一点，操作数是对象，将对象转换为原始值。

两边对象都是数组，左边的数组先调用 valueOf() 方法无果，然后去调用 toString(), 方法，在 toString() 的转化规则里面有『将数组转化为字符串，用逗号分隔』，由于没有其他元素，所以直接是空字符串 “”。




	{}+[]//0


	[]+{}//"[object Object]"




# svg 制作loading



	HTML代码：
	circle {
	    -webkit-transition: stroke-dasharray .25s;
	    transition: stroke-dasharray .25s;
	}
	HTML代码：
	<svg width="440" height="440" viewbox="0 0 440 440">
	    <circle cx="220" cy="220" r="170" stroke-width="50" stroke="#D1D3D7" fill="none"></circle>
	    <circle cx="220" cy="220" r="170" stroke-width="50" stroke="#00A5E0" fill="none" transform="matrix(0,-1,1,0,0,440)" stroke-dasharray="0 1069"></circle>
	</svg>
	<p>拖我：<input id="range" type="range" min="0" max="100" value="0" style="width:300px;"></p>
	JS代码：
	if (window.addEventListener) {
	    var range = document.querySelector("#range"), circle = document.querySelectorAll("circle")[1];
	    if (range && circle) {
	        range.addEventListener("change", function() {
	            var percent = this.value / 100, perimeter = Math.PI * 2 * 170;
	            circle.setAttribute('stroke-dasharray', perimeter * percent + " " + perimeter * (1- percent));
	        });
	    }
	}

[https://www.zhangxinxu.com/wordpress/2015/07/svg-circle-loading/](https://www.zhangxinxu.com/wordpress/2015/07/svg-circle-loading/)




    circle {
        -webkit-transition: stroke-dasharray .25s;
        transition: stroke-dasharray .25s;
    }

    .spin{
        animation: ani-demo-spin 700ms linear infinite;
        transform-origin:center center;
    }
    @keyframes ani-demo-spin {
        from { transform: rotate(0deg);}
        50%  { transform: rotate(180deg);}
        to   { transform: rotate(360deg);}
    }

配上css


# 获取滚动条的方法


        let noScroll, scroll, oDiv = document.createElement("DIV");
        oDiv.style.cssText = "position:absolute; top:-1000px; width:100px; height:100px; overflow:hidden;";
        noScroll = document.body.appendChild(oDiv).clientWidth;
        oDiv.style.overflowY = "scroll";
        scroll = oDiv.clientWidth;
        document.body.removeChild(oDiv);
        this.gutterWidth=noScroll - scroll;
