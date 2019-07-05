---
title: thinkphp的一些标签
date: 2017-05-02 13:41:16
tags:
---
# 前言

用于记录一些项目中常用的thinkphp的标签


<!--more--><!--more-->



# 查看thinkphp版本号


	ThinkPHP\Common\runtime.php 第22行有版本信息 ，如：
	define('THINK_VERSION', '3.0');

	
	3.2rc版本中，版本常量的定义被移到了ThinkPHP.php

## 操作url的方法

	I('变量类型.变量名',['默认值'],['过滤方法'])
	http://www.thinkphp.cn/document/308.html

	echo I('get.id'); // 相当于 $_GET['id']
	echo I('get.name'); // 相当于 $_GET['name']

## Empty标签

判断模板变量是否为空

	<empty name="name">name为空值</empty>

	
	<notempty name="name">name不为空</notempty>


	<empty name="name">name为空<else /> name不为空</empty>


empty当name为空的时候才显示，notempty是name不为空的时候显示


	<notempty name="comment['content']">


	<if condition="$comment['content']">

	<if condition="trim($comment['content'])">用于除去内容中的空格


查看那么的参数：


	<notempty name="info['comments']">


	<pre><?php var_dump($info['comments'])?></pre>



	先将array转化为json，console.log，比上面看着舒服
    var str=<?php echo(json_encode($info['comments']))?>;
    console.log(str);



也可以在对应的controller里的public function index()

	添加$copyrightSupport = json_encode($data['copyrightSupport']);

然后在对应的网页里获取
	var data = {$copyrightSupport};
	console.log(data);


php中


	var_dump() 能打印出类型
	print_r() 只能打出值
	echo() 是正常输出...


数组的形式：


	【[name1]=>array(1),[name2]=>array(2)】

	调用数组：
	array【name1】即可


## volist用于循环

	<pre><?php var_dump($list)?></pre>  //查看数组

   index值为$i


![](http://i.imgur.com/cwgcfQi.png)

还想查看其中的数据：例如套餐

	<pre><?php var_dump($list[0]["name"])?></pre>

	注意汉字1个字符三个汉字




	<volist name="list" id="vo">
    //相当于  list.each(vo) 或者  for vo in list
		{$vo.id}
		{$vo.name}
	</volist>


	$User = M('User');
	$list = $User->select();
	$this->assign('list',$list);



支持输出部分数据，例如输出其中的第5～15条记录
	<volist name="list" id="vo" offset="5" length='10'>
		{$vo.name}
	</volist>





输出偶数记录
	<volist name="list" id="vo" mod="2" >
		<eq name="mod" value="1">{$vo.name}</eq>
	</volist>


输出key值：
	<volist name="list" id="vo" mod="2" >
		{$key}
	</volist>


	<foreach name="arr" item="vo">
	    <p>{$vo}------ {$key}</p>
	</foreach>

	<for start="1" end="10" comparison="elt" name="arr">
	    <p>{$arr}</p>
	</for>





## if用于判断


	<if condition="($name eq 1) OR ($name gt 100) "> value1
	<elseif condition="$name eq 2"/>value2
	<else /> value3
	</if>



eg:

	class="select <if condition="ceil(count($film_class)/7) eq 1">none</if>"

	


condition是判断条件



## {:U('index/profile',array('id'=>$vo['id']))}

U函数是tp里面特有的一个函数，作用是设置url链接。函数里面第一个参数是URL表达式，第二个参数是url带的参数。
这个地址经过u函数的解析后会返回一个链接：

	index/profile?id=xxx

eg:

	{:U('details',array('id'=>$vo['id']))}
	$vo['id']=>int(1)


	details/id/1


## strpos() 函数

查找字符串在另一字符串中第一次出现的位置，函数对大小写敏感


	<?php
	echo strpos("You love php, I love php too!","php");
	?>

	结果为9，从0开始结果为9,如何没有结果返回为false


## {:numFormat($vo['lbsdistance']/1000,1)}

number_format() 函数通过千位分组来格式化数字。


	echo number_format("5000000")."<br>";
	echo number_format("5000000",2)."<br>";
	echo number_format("5000000",2,",",".");

结果为：

	5,000,000
	5,000,000.00
	5.000.000,00


##  比较标签


	<比较标签 name="变量" value="值">内容</比较标签>

	eq或者 equal	等于
	neq 或者notequal	不等于
	gt	大于
	egt	大于等于
	lt	小于
	elt	小于等于
	heq	恒等于
	nheq	不恒等于

	<eq name="name" value="value">value</eq>

	求name变量的值等于value就输出


#   页面对应的控制器编写

     

	找到对应的编辑器，controller中的文件


	public function movie(){  //不改变文件名称
        $this->display();
    }

    public function test(){   //改变文件名称
        $this->display('movie');
    }


	$this->display('Member:read'); //跳转到其他控制器



	$page = I('request.page', 2, 'intval');
     
	request:ajax传过来的json
	request.page，json.page   默认值为2     变成数位整形



# 获取数组的长度


	count($data[copyrightSupport]) elt 2
    当数组小于等于2的时候



# 限制一些输出的字长


    {$res['drama_cn']}

    {:msubstr($res['drama_cn'], 0, 90)}



# 第一个项加class名


    <volist name="list[data_list]" id="vo">
        <div class="swiper-slide two-overflow <eq name='key' value='0'>active</eq>"  data-movieid="{$vo.id}"  data-video_source="{$vo.video_source}">
            {$vo.title}
        </div>
    </volist>


# 根据url链接来显示状态

     url中存在？type=1

    <if condition="I('get.type') EQ 1">
        <a href="/Customer/message/type/1.html" class="on playMsg">播放记录</a>
        <else/>
        <a href="/Customer/message/type/1.html" class="playMsg">播放记录</a>
    </if>

    <if condition="I('get.type') EQ 2">
        <a href="/Customer/message/type/2.html" class="on userMsg">购买记录</a>
        <else/>
        <a href="/Customer/message/type/2.html" class="userMsg">购买记录</a>
    </if>

    <if condition="I('get.type') EQ 3">
        <a href="/Customer/message/type/3.html" class="on userMsg">消息</a>
        <else/>
        <a href="/Customer/message/type/3.html" class="userMsg">消息</a>
    </if>


# 本页

    <a href="{:U()}">testtest</a>


 	 U("模块/方法",array(参数=值，参数=值)，"html",true/false 是否跳转，域名)；


# 获取url中的值


    <if condition="$Think.get.show_detail eq 1">



# 循环的条件为空

	<if condition="!empty($VideoHallList)">


# URL生成

使用U方法

U('地址表达式',['参数'],['伪静态后缀'],['显示域名'])


	U('User/add') // 生成User控制器的add操作的URL地址
	U('Blog/read?id=1') // 生成Blog控制器的read操作 并且id为1的URL地址
	U('Admin/User/select') // 生成Admin模块的User控制器的select操作的URL地址



	U('Blog/cate',array('cate_id'=>1,'status'=>1))
	U('Blog/cate','cate_id=1&status=1')
	U('Blog/cate?cate_id=1&status=1')


但是不允许使用下面的定义方式来传参数
	U('Blog/cate/cate_id/1/status/1');


伪静态后缀

	U('Blog/cate','cate_id=1&status=1','xml');


## ajax返回

数据

	$this->ajaxReturn($data,'xml'); //默认为json

正确


	// 操作完成3秒后跳转到 /Article/index
	$this->success('操作完成','/Article/index',3);
	// 操作失败5秒后跳转到 /Article/error
	$this->error('操作失败','/Article/error',5);



##  获取数据



I('变量类型.变量名/修饰符',['默认值'],['过滤方法'],['额外数据源'])


	echo I('get.id'); // 相当于 $_GET['id']
	echo I('get.name'); // 相当于 $_GET['name']

	
	
	I('post.name','','htmlspecialchars'); // 采用htmlspecialchars方法对$_POST['name'] 进行过滤，如果不存在则返回空字符串
	I('session.user_id',0); // 获取$_SESSION['user_id'] 如果不存在则默认为0
	I('cookie.'); // 获取整个 $_COOKIE 数组
	I('server.REQUEST_METHOD'); // 获取 $_SERVER['REQUEST_METHOD'] 


变量过滤



	// 系统默认的变量过滤机制
	'DEFAULT_FILTER'        => 'htmlspecialchars'


	// 等同于 htmlspecialchars($_GET['name'])
	I('get.name'); 


变量修饰符


	例如：
		I('get.id/d');
		I('post.name/s');
		I('post.ids/a');

		修饰符	作用
		s	强制转换为字符串类型
		d	强制转换为整形类型
		b	强制转换为布尔类型
		a	强制转换为数组类型
		f	强制转换为浮点类型


## 判断请求类型


	IS_GET	判断是否是GET方式提交
	IS_POST	判断是否是POST方式提交
	IS_PUT	判断是否是PUT方式提交
	IS_DELETE	判断是否是DELETE方式提交
	IS_AJAX	判断是否是AJAX提交
	REQUEST_METHOD	当前提交类型