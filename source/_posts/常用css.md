---
title: 常用css
date: 2017-05-02 09:05:33
tags: css
---



# 前言

工作上常遇见的一些css方面的问题，特意列出





<!--more--><!--more-->



#  样式清空


	/* 清除内外边距 */
	body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, /* structural elements 结构元素 */
	dl, dt, dd, ul, ol, li, /* list elements 列表元素 */
	pre, /* text formatting elements 文本格式元素 */
	fieldset, lengend, button, input, textarea, /* form elements 表单元素 */
	th, td { /* table elements 表格元素 */
	    margin: 0;
	    padding: 0;
	}
	
	/* 设置默认字体 */
	body,
	button, input, select, textarea { /* for ie */
	    /*font: 12px/1 Tahoma, Helvetica, Arial, "宋体", sans-serif;*/
	    font: 12px/1 Tahoma, Helvetica, Arial, "\5b8b\4f53", sans-serif; /* 用 ascii 字符表示，使得在任何编码下都无问题 */
	}
	
	h1 { font-size: 18px; /* 18px / 12px = 1.5 */ }
	h2 { font-size: 16px; }
	h3 { font-size: 14px; }
	h4, h5, h6 { font-size: 100%; }
	
	address, cite, dfn, em, var { font-style: normal; } /* 将斜体扶正 */
	code, kbd, pre, samp, tt { font-family: "Courier New", Courier, monospace; } /* 统一等宽字体 */
	small { font-size: 12px; } /* 小于 12px 的中文很难阅读，让 small 正常化 */
	
	/* 重置列表元素 */
	ul, ol { list-style: none; }
	
	/* 重置文本格式元素 */
	a { text-decoration: none; }
	a:hover { text-decoration: underline; }
	
	abbr[title], acronym[title] { /* 注：1.ie6 不支持 abbr; 2.这里用了属性选择符，ie6 下无效果 */
	border-bottom: 1px dotted;
	cursor: help;
	}
	
	q:before, q:after { content: ''; }
	
	/* 重置表单元素 */
	legend { color: #000; } /* for ie6 */
	fieldset, img { border: none; } /* img 搭车：让链接里的 img 无边框 */
	/* 注：optgroup 无法扶正 */
	button, input, select, textarea {
	    font-size: 100%; /* 使得表单元素在 ie 下能继承字体大小 */
	}
	
	/* 重置表格元素 */
	table {
	border-collapse: collapse;
	border-spacing: 0;
	}
	
	/* 重置 hr */
	hr {
	    border: none;
	    height: 1px;
	}


# 常用的公共css

	¥  &yen;   //人民币符号


	.text-overflow{   //文字溢出   宽度自己定义
	  white-space:nowrap;   //不换行     都支持
	  text-overflow:ellipsis;
	  -o-text-overflow:ellipsis;   //省略号显示   都支持
	  overflow: hidden;
	}
	.none{
	  display: none;
	}
	.hidden{
	  visibility: hidden;
	}
	
	.relative{
	  position: relative;
	}
	.y-center{       //配合relative，父元素垂直居中     IE9及其以上
	  position: absolute;
	  top: 50%;
	  transform: translateY(-50%);
	  -ms-transform:translateY(-50%); 	/* IE 9 */
	  -moz-transform:translateY(-50%);	/* Firefox */
	  -webkit-transform:translateY(-50%);/* Safari 和 Chrome */
	  -o-transform:translateY(-50%); 	/* Opera */
	}
	.xy-center{//配合relative，父元素水平垂直居中     IE9及其以上
	  position: absolute;
	  top: 50%;
	  left: 50%;
	  transform: translate(-50%,-50%);
	  -ms-transform:translate(-50%,-50%); 	/* IE 9 */
	  -moz-transform:translate(-50%,-50%);	/* Firefox */
	  -webkit-transform:translate(-50%,-50%);/* Safari 和 Chrome */
	  -o-transform:translate(-50%,-50%); 	/* Opera */
	}
	.clearfix:after{    //清除浮动  利用的是伪元素
	  content: "\0020";
	  display: block;
	  height: 0;
	  clear: both;
	  visibility: hidden;
	}
	.clearfix{
	  zoom: 1;
	}


# 文本不可以选中：

        -moz-user-select:none;/*火狐*/
        -webkit-user-select:none;/*webkit浏览器*/
        -ms-user-select:none;/*IE10*/
        -khtml-user-select:none;/*早期浏览器*/
        user-select:none;

# 苹果文本框的黑框问题

	input{
		outline:none; 
		-webkit-appearance: none;
		-webkit-tap-highlight-color: rgba(0,0,0,0);
	}

# textarea文本框禁止拉伸

 	textarea{
		resize:none
	}

# 文本框溢出隐藏，以逗号省略

	
	width:200px; 
	white-space:nowrap;   //不换行     都支持
	text-overflow:ellipsis; 
	-o-text-overflow:ellipsis;   //省略号显示   都支持
	overflow: hidden;



两行：
 
    //兼容性有问题  推荐在手机端上使用   PC端让后台限制

    word-break: break-all;
    text-overflow: ellipsis;
    display: -webkit-box; /** 对象作为伸缩盒子模型显示 **/
    -webkit-box-orient: vertical; /** 设置或检索伸缩盒对象的子元素的排列方式 **/
    -webkit-line-clamp: 2; /** 显示的行数 **/
    overflow: hidden;  /** 隐藏超出的内容 **/




# 内容超出滚动条

    width: 200px;
    height: 100px;
    overflow: auto;

不过水平滚动条影响用户体验,推荐使用即可

	overflow-y


# +选择器

	就是选择某个元素的下一个元素
	<p id="x"><p>
	<p></p>
	
	#x + p 就是选择第二个P

# !important无效

	background-color: red;!important;

   	正确：background-color: red!important;



# 背景蒙层的设置

        position: fixed;
        z-index: 1000;
        top: 0;
        right: 0;
        left: 0;
        bottom: 0;
        background: rgba(0, 0, 0, 0.6);  


# css的水平居中

	top: 50%(让元素的上边界定位到父元素的50%位置)
	transform: translateY(-50%)(让元素往上移动元素自身高度的50%)

同理垂直居中：

	  top: 50%;
	  transform: translateY(-50%);


兼容性：ie10，加前缀到ie9


        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        -ms-transform:translateY(-50%); 	/* IE 9 */
        -moz-transform:translateY(-50%);	/* Firefox */
        -webkit-transform:translateY(-50%);/* Safari 和 Chrome */
        -o-transform:translateY(-50%); 	/* Opera */




不考虑兼容性的做法：


	.father{
		display: table;
	}
	.son{
	    text-align: center;
	    vertical-align: middle;
	    display: table-cell;
		/*=============不能在contect的div 设置float属性 复制无效，应该设置在class中===========
	}



水平垂直居中+左固定，右自适应

	@width:800px;
	.box{
	  height: 500px;
	  position: relative;
	  overflow: hidden;
	    .left{
	        float: left;
	        width: @width;
	        height: 100%;
	        background-color: #CCC;
	    }
	    .right{
	        background-color: pink;
	        height: 100%;
	        min-width: 150px;
	        margin-left: @width;
	        position: relative;
	        .rBox{
	            background-color: red;
	            position: absolute;
	            top: 50%;
	            left: 50%;
	            transform: translate(-50%,-50%);
	        }
	  }
	}


儿子已知宽高的垂直居中

	  position: relative;
	    .rBox{
	        background-color: red;
	        position:absolute;
	        top:0;
	        left: 0;
	        right:0;
	        bottom:0;
	        margin:auto;
	        width:300px;
	        height:300px;
	    }




# css的隐藏元素，无需js代码

	transform: translateY(100%)隐藏了起来
	显示时用translateY(0). 



# uc浏览器不支持transform

用-webkit-transform来代替


# input number的默认样式的修改


    input[type=number] {
        -moz-appearance:textfield;
    }
    input[type=number]::-webkit-inner-spin-button,
    input[type=number]::-webkit-outer-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }



# 禁止复制


	body {  
	    -webkit-touch-callout: none;  
	    -webkit-user-select: none;  
	    -khtml-user-select: none;  
	    -moz-user-select: none;  
	    -ms-user-select: none;  
	    user-select: none;  
	}


	<script type="text/javascript">
	document.oncontextmenu=function(e){return false;}
	</script>  
	<body onselectstart="return false">


# 在title上添加logo
	
	<link rel="SHORTCUT ICON" href="http://网址路径/favicon.ico">


	<link rel="shortcut icon" href="favicon.ico">

#  颜色透明的英文

	transparent


# 移动端常见的medta

	weui在加载js和css文件后一般不会生效，要加上对应的meta才行。

	<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=0">

viewport：可视区域的宽度   通过innerWidth和Height获取


device-width：手机屏幕分辨率适合的宽度，不同的设备宽度不一样


intial-scale：页面首次被显示是可视区域的缩放级别，取值1.0则页面按实际尺寸显示，无任何缩放

user-scalable:是否可对页面进行缩放，no 禁止缩放


动态改变meta标签的方式：

	document.write('<meta name="viewport" content="width=device-width,initial-scale=1">')

	<meta id="testViewport" name="viewport" content="width = 380">
	<script>
	var mvp = document.getElementById('testViewport');
	mvp.setAttribute('content','width=480');
	</script>


# meta禁止缓存

	<meta content="no-cache,must-revalidate" http-equiv="Cache-Control">

no-cache: 告诉浏览器、缓存服务器，不管本地副本是否过期，使用资源副本前，一定要到源服务器进行副本有效性校验。


must-revalidate：告诉浏览器、缓存服务器，本地副本过期前，可以使用本地副本；本地副本一旦过期，必须去源服务器进行有效性校验。



禁用客户端缓存
HTM网页
	<META HTTP-EQUIV="pragma" CONTENT="no-cache">
	<META HTTP-EQUIV="Cache-Control" CONTENT="no-cache, must-revalidate">
	<META HTTP-EQUIV="expires" CONTENT="Wed, 26 Feb 1997 08:21:57 GMT">



# 去掉input="number"的默认样式


	input[type=number] {  
	    -moz-appearance:textfield;  
	}  
	input[type=number]::-webkit-inner-spin-button,  
	input[type=number]::-webkit-outer-spin-button {  
	    -webkit-appearance: none;  
	    margin: 0;  
	}  


# 下拉滚动条隐藏但是可以滚动


推荐方法：思路是一个dom包含一个子dom，子dom比父宽17px，父dom溢出隐藏.可以查看一下滚动条的宽度


	<div class="box1">
	    <div class="box" ></div>
	</div>

    .box{
        width: 318px;
        height: 250px;
        overflow-x: hidden;
        overflow-y: auto;
    }
    .box1{
        margin: 200px auto;
        width: 300px;
        height: 250px;
        overflow: hidden;
    }


	//webkit内核的浏览器
	::-webkit-scrollbar{
	  display:none;
	}


	//ie可以  
	html {
	    -ms-overflow-style:none;
	    overflow:-moz-scrollbars-none;
	}
	html::-webkit-scrollbar{width:0px}


#  去掉聚焦在input框上时产生的发光轮廓

    input:focus{outline:0 !important}


# 谷歌浏览器最小字体为12px的解决：

	-webkit-transform-origin-x: 0;//解决缩放后margin-left的位移问题

	-webkit-transform: scale(0.5);   //缩放让字体变小


	.small-font{
	    font-size:12px;
	
	    transform: scale(0.90);
	    transform-origin:0 0;
	
	    -ms-transform: scale(0.90);         /* IE 9 */
	    -ms-transform-origin:0 0;       /* IE 9 */
	
	    -webkit-transform: scale(0.90); /* Safari 和 Chrome */
	    -webkit-transform-origin:0 0;   /* Safari 和 Chrome */
	
	    -moz-transform: scale(0.90);        /* Firefox */
	    -moz-transform-origin:0 0;      /* Firefox */
	
	    -o-transform: scale(0.90);      /* Opera */
	    -o-transform-origin:0 0;        /* Opera */
	}


# box-shadow兼容ie8

	-moz-box-shadow:3px 5px 5px #969696;
	-webkit-box-shadow:3px 5px 5px #969696;
	box-shadow:3px 5px 5px #969696;
	 filter: progid:DXImageTransform.Microsoft.Shadow(color='#969696', Direction=125, Strength=9);



# 三行省略   兼容ie8


	
	.two-row-css{
	  position:relative;
	  line-height:1.4em;
	  /* 3 times the line-height to show 3 lines */
	  height:2.8em;
	  overflow:hidden;
	}
	.two-row-css:after{
	  content:"...";
	  display: block;
	  width: 10px;
	  height: 19px;
	  background-color: @white;
	  font-weight:bold;
	  position:absolute;
	  bottom:0;
	  right:0;
	}、
配合js：
    $(function () {
        var heigh=$(".movie-time").height();   //单行的高度
        var $twoRow=$(".two-row");
        for(var i=0;i<$twoRow.size();i++){    //超过两倍高度的时候显示两行....
            ($twoRow.eq(i).height()>2*heigh)&&($twoRow.eq(i).addClass("two-row-css"));
        }
    })


# ie margin auto失效  


	body{window：100%；text-align:center;}



# html5视频填满整个dom

	object：fit；


	
	fill: 中文释义“填充”。默认值。替换内容拉伸填满整个content box, 不保证保持原有的比例。
	contain: 中文释义“包含”。保持原有尺寸比例。保证替换内容尺寸一定可以在容器里面放得下。因此，此参数可能会在容器内留下空白。
	cover: 中文释义“覆盖”。保持原有尺寸比例。保证替换内容尺寸一定大于容器尺寸，宽度和高度至少有一个和容器一致。因此，此参数可能会让替换内容（如图片）部分区域不可见。
	none: 中文释义“无”。保持原有尺寸比例。同时保持替换内容原始尺寸大小。
	scale-down: 中文释义“降低”。就好像依次设置了none或contain, 最终呈现的是尺寸比较小的那个。



# 中文换行，英文不换行


适用于table的宽度设定好，但是没有自动换到导致整个table布局破坏


	1. word-break:break-all;只对英文起作用，以字母作为换行依据
	2. word-wrap:break-word; 只对英文起作用，以单词作为换行依据


中英文都换行：

      white-space:pre-wrap;
      word-break:break-all;



其他：


	1. word-break:break-all;只对英文起作用，以字母作为换行依据
	2. word-wrap:break-word; 只对英文起作用，以单词作为换行依据
	3.white-space:pre-wrap; 只对中文起作用，强制换行
	4.white-space:nowrap; 强制不换行，都起作用
	5.white-space:nowrap; 
	overflow:hidden; 
	text-overflow:ellipsis;不换行，超出部分隐藏且以省略号形式出现（部分浏览器支持）






# li下面的3px间距

解决方法是给li 添加一个css {vertical-align:bottom;} 无需hack


# 禁止选择文本


   -moz-user-select: -moz-none;
   -khtml-user-select: none;
   -webkit-user-select: none;
   -ms-user-select: none;
   user-select: none;


# hover点击以后失效


	元素执行完click事件后hover效果就失效问题
	
	css 里面加“ !important”可以提高优先级。
	.a1:hover{backgroundcolor:red !important;}


   或者两个类名，用js来切换


# 两边固定，中间自适应

	#left,#right { float: left; width: 220px; height: 200px; background: blue;} 
	
	#right { float: right;} 
	
	#main { margin: 0 230px;background: red; height: 200px;}

这种方法我利用的就是浮动原理，左右定宽度分别进行左浮动和右浮动，此时主内容列（中间列没有定度）主会自动插入到左右两列的中间，最要注意的一点是，中间列一定要放在左右两列的后面



# 上下固定，中间自适应

  利用postion：a的自动填充

	.header{ 
	position:absolute; 
	top:0px; 
	height:268px;/*高度可以不写，可以通过内部元素撑开，但是需要考虑是否会与自适应部分发生重合*/ 
	width: 182px;/*宽度是必要，如果没有宽度就无法撑出div*/ 
	} 
	.middle_outer{ 
	position:absolute!important; 
	position:relative; 
	top:268px!important;/*header部分的高度*/ 
	top:0; 
	bottom:52px;/*footer部分的高度*/ 
	width:182px; 
	overflow:hidden;/*外层div不滚动，而是内层div滚动，实现自适应*/ 
	height:auto!important; 
	height:100%; 
	} 
	.middle_inner{ 
	height:100%; 
	overflow-y:auto;/*当内容超出后，就会出现滚动条*/ 
	} 


	<div class='header'></div> 
	<div class='middle_outer'> 
	<div class='middle_inner'></div> 
	</div> 
	<div class='footer'></div> 






# 大于符号>


	.gl{//大于号样式
	  width: 8px;
	  height: 8px;
	  position: absolute;
	  border-left: 1px solid #999;
	  border-bottom: 1px solid #999;
	  -webkit-transform: translate(0,-50%) rotate(-135deg);
	  transform: translate(0,-50%) rotate(-135deg);
	}


#  placeholder的颜色

	/*=====placeholder的颜色=====*/
	::-webkit-input-placeholder{color: #c1c1c4}
	:-moz-placeholder          {color: #c1c1c4}
	::-moz-placeholder         {color: #c1c1c4}
	:-ms-input-placeholder     {color: #c1c1c4}


# footer固定在最下面

	<body>
	    <header>header</header>
	    <main>main content</main>
	    <footer>footer</footer>
	</body>



	html{height:100%;}
	body{min-height:100%;margin:0;padding:0;position:relative;}
	main{padding-bottom:100px;}

	/* main的padding-bottom值要等于或大于footer的height值 */

	footer{position:absolute;bottom:0;width:100%;height:100px;}


# 清除浮动加上后dom高度无故变高


	使用的是左固定右自适应的布局
	在右的容器上加上
	margin-left：xxxpx 上加上overflow:hidden解决;



# 滚动条

	::-webkit-scrollbar-thumb {
	  /* height: 60px; */
	  background-color: #636871;
	  -webkit-border-radius: 10px;
	  outline: #c6ced7 solid 2px;
	  outline-offset: -2px;
	  border: 2px solid #c6ced7;
	}
	::-webkit-scrollbar-track-piece {
	  background-color: #c6ced7;
	  -webkit-border-radius: 0;
	}
	::-webkit-scrollbar {
	  width: 9px;
	  height: 9px;
	}


# 常用的框架布局



	html,body,.app{
	  width: 100%;
	  height: 100%;
	  background-color: #e9ecf3!important;
	}
	.header{
	  width: 100%;
	  height: 50px;
	  background-color: red;
	}
	.container{
	  width: 100%;
	  position: absolute;
	  top: 50px;
	  bottom: 0;
	  .aside{
	    background-color: pink;
	    height: 100%;
	    width: 200px;
	    float: left;
	  }
	  .main-container{
	    height: 100%;
	    margin-left: 200px;
	    position: relative;
	    .main{
	      width: 100%;
	      position: absolute;
	      top: 0;
	      bottom: 30px;
	      background-color: orange;
	    }
	    .foot{
	      width: 100%;
	      height: 30px;
	      position: absolute;
	      bottom: 0;
	      background-color: blue;
	    }
	  }
	}




	<div id="app" v-cloak class="app">
	    <div class="header">
	
	    </div>
	    <div class="container">
	        <div class="aside">
	
	        </div>
	        <div class="main-container">
	            <div class="main">
	
	            </div>
	            <div class="foot">
	
	            </div>
	        </div>
	    </div>
	</div>


# 移动设备上 宽高一样自适应


	.ui-square {
	  width: 20%;
	  height: 0;
	  padding-bottom: 20%;
	  background: blue;
	}



# 数字排列


    .one {
        margin: 0 auto;
        height: 193px;
        line-height: 2;
        font-size: 24px;
        writing-mode: vertical-lr;/*从左向右 从右向左是 writing-mode: vertical-rl;*/
        writing-mode: tb-lr;/*IE浏览器的从左向右 从右向左是 writing-mode: tb-rl；*/
    }


# html5  视频隐藏进度条


	video::-webkit-media-controls {
	    display:none !important;
	}


	video::-webkit-media-controls-enclosure {
	    /*禁用播放器控制栏的样式*/
	    display: none !important;
	}



# 渐变

	.gradient{
         background:-moz-linear-gradient(top, #fffab2, #ffc343)!important;
         background:-webkit-gradient(linear, 0 0, 0 bottom, from(#fffab2), to(#ffc343))!important;
         background:-o-linear-gradient(top, #fffab2, #ffc343)!important;



        background: -moz-linear-gradient(top, #fffab2 0%, #ffc343 100%);
        background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,#fffab2 0%), color-stop(100%,#ffc343));
        background: -webkit-linear-gradient(top, #fffab2 0%,#ffc343 100%);
        background: -o-linear-gradient(top, #fffab2 0%,#ffc343 100%);
        background: -ms-linear-gradient(top, #fffab2 0%,#ffc343 100%);
        background: linear-gradient(to bottom, #fffab2 0%,#ffc343 100%);
        filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#fffab2', endColorstr='#ffc343',GradientType=0 );
	}




# 常用的符号


## 关闭符号

	.close{
	  position: relative;
	  width: 50px;
	  transform:rotate(-45deg);
	  display: flex;
	  flex-direction: column;
	  justify-content:center;
	  align-items:flex-end;
	  margin-left: 40px;
	  box-sizing: border-box;
	  padding-top: 19px;
	}
	.close::after,.close::before{
	  content: '';
	  display: block;
	  width: 30px;
	  border-top: 5px solid #333;
	  position: absolute;
	}
	.close::after{
	}
	.close::before{
	  transform:rotate(-90deg);
	}


#  下三角

	.arrow-down{
	  content: '';
	  width: 20px;
	  height: 20px;
	  border-right:5px solid #cbcbcb;
	  border-bottom: 5px solid #cbcbcb;
	  display: block;
	  position: absolute;
	  top: 50%;
	  transform:rotate(45deg)  translateY(-50%);
	}


# next和prev



    .next,.prev{
        width: 20px;
        background-color: pink;
        float: left;
        position: relative;
    }
    .next:after,.prev:after{
        content: '';
        display: block;
        width: 10px;
        height: 10px;
        border-left: 2px solid #999999;
        border-bottom: 2px solid #999999;
        position: absolute;
        z-index: 9999;
        top: 50%;

    }
    .next:after{
        transform:rotate(-135deg);
        left: 20%;
    }
    .prev:after{
        transform:rotate(45deg);
        left: 50%;
    }


#   常见符号

	 &lt;  	 < 	 小於号或显示标记    
	 &gt;	 >	 大於号或显示标记
	 &amp;   	 &	 可用於显示其它特殊字符  
	 &quot;	 "	 引号
	 &reg;	 ®	 己注册
	 &copy; 	 ©	 版权
	 &trade;	 ™	 商标
	 &ensp;	 	 半方大的空白
	 &emsp;	 	 全方大的空白
	 &nbsp;	 	 不断行的空白



# table的文字溢出问题

	
	table{
	
	　　width:100px;
	
	　　table-layout:fixed;/* 只有定义了表格的布局算法为fixed，下面td的定义才能起作用。 */
	
	}
	
	td{
	
	　　width:100%;
	
	　　word-break:keep-all;/* 不换行 */
	
	　　white-space:nowrap;/* 不换行 */
	
	　　overflow:hidden;/* 内容超出宽度时隐藏超出部分的内容 */
	
	　　text-overflow:ellipsis;/* 当对象内文本溢出时显示省略标记(...) ；需与overflow:hidden;一起使用*/
	
	}




# flex属性的新认识


	flex是flex-grow、flex-shrink和flex-basis三个属性的速记


	li { flex: 0 1 auto; }

	li { flex-grow: 0;
	    flex-shrink: 1;
	    flex-basis: auto; }


1. flex-grow 属性用于设置或检索弹性盒子的扩展比率



2. flex-shrink的默认值为1，如果没有显示定义该属性，将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。主要是宽度没有默认的宽度那么大时，缩放时的大小比例

3. flex-basis 属性用于设置或检索弹性盒伸缩基准值，可以用来设置固定的宽度





## flex-wrap

控制其内部的元素是否可以换行


nowrap 不可以换行
wrap   可以换行，正顺序
wrap-reverse   可以换行，逆序


# 修改光标的颜色

       caret-color:#2198f0;  

兼容性：IE 11 苹果不兼容




# 多列均匀分布


	text-align:justify + text-align-last

但是

	text-align-last兼容性差


	text-align:justify
	
	（文字内容要超过一行）


兼容IE6+需要用

        
    .justify{
        text-align:justify;
        background-color: pink;
        font-size: 0;  //清空素质方向的多于高度
    }
    .justify div{
        width: 22%;
        height: 50px;
        font-size: 14px;
        display: inline-block;
        background-color: beige;
        outline: 1px solid #2BD6A4;
    }

    .justify:after {
        display:inline-block;
        width:100%;
        height:0;
        content:'';
    }



	<div class="justify">
	    <div>1</div>
	    <div>2</div>
	    <div>3</div>
	    <div>4</div>
	    <div>5</div>
	    <div>6</div>
	    <div>7</div>
	    <div>8</div>
	    <div>9</div>
	    <div>5</div>
	    <div>5</div>
	</div>


# 苹果手机点击输入框时页面自动放大
	
	<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
	
	** user-scalable=no是“禁止手动缩放”，添加此属性后便不可手动控制页面大小。



# 毛玻璃


	
	.background_img{ 
	  position: fixed;
	  top: 0;
	  left: 0;
	  bottom: 0;
	  width: 100%;
	  height: 100%;
	  filter: blur(20px);
	  z-index: -1;
	  transform: scale(1.5); /*和网易云音乐对比了一下，发现也是放大1.5倍*/
	}


	<!-- play.wxml -->
	<image src="{{song.al.picUrl}}" class="background_img" ></image>
