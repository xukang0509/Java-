# 互联网通信流程

1. 学习任务：掌握互联网通信流程

2. 学习特点
   - 【背】互联网通信流程中每一个细节
   - 本阶段使用命令都是老旧命令，不需要记忆。
   
3. 学习要求
   - 一定要背互联网通信流程细节
   - 多多交流

4. 涉及到的技术【老旧】
   - 控制浏览器行为技术：HTML、CSS、JavaScript
   - 控制硬盘上数据库行为技术：MySql数据库服务器管理使用（SQL重点），JDBC规范
   - 控制服务器端Java行为技术：Http服务器、Servlet、JSP
   - 互联网通信流程中开发规则：MVC
   - 贯穿的项目-----在线考试管理系统

5. 什么是互联网通信？
   
- 两台计算机通过网络实现文件共享行为，就是【互联网通信】
  
6. 互联网通信过程角色划分

   - 客户端计算机：用于发送请求，来索要资源文件的计算机
   - 服务端计算机：用于接收请求，并提供对应资源文件的计算机

7. 互联网通信的模型

   1. C/S通信模型

      - C，client software，客户端软件

      ```
      客户端软件专门安装在客户端计算机上
      帮助客户端计算机向指定服务端计算机发送请求，索要资源文件
      帮助客户端计算机将服务端计算机发送回来【二进制数据】解析为【文字，数字，图片，视频，命令】
      ```

      - S，server software，服务器软件

      ```
      服务器软件专门安装在服务端计算机上
      服务器软件用于接收来自于特定的客户端软件发送请求
      服务器软件在接收到请求之后自动的在服务端计算机上定位被访问的资源文件
      服务器软件自动的将定位的文件内容解析为【二进制数据】通过网络发送回发起请求的客户端软件上
      ```

      - 适用场景

      ```
      C/S通信模型普遍用于个人娱乐市场，比如【微信，淘宝/京东，视频（优酷/B站），大型网络游戏(魔兽/英雄联盟)】
      企业办公领域相对应用较少
      ```

      - C/S通信模型优缺点

      ```
      优点：
      	1.安全性较高；
      	2.有效降低服务端计算机工作压力
      缺点：
      	1.增加客户获得服务的成本；
      	2.更新较为繁琐
      ```

   2. B/S通信流程

      - B，browser，浏览器

      ```
      浏览器安装在客户端计算机软件
      可以向任意服务器发送请求，索要资源文件
      可以将服务器返回的【二进制数据】解析为【文字，数字，图片，视频，命令】
      ```

      - S，server software，服务器软件

      ```
      服务器软件专门安装在服务端计算机上
      可以接收任意浏览器发送请求
      自动的在服务端计算机上定位被访问的资源文件
      自动的将定位的资源文件内容以二进制形式发送回发起请求浏览器上
      ```

      - 适用场景：既适用于个人娱乐市场，又广泛适用于企业日常活动
      - B/S通信模型优缺点

      ```
      优点：
      	1.不会增加用户获得服务的成本
      	2.几乎不需要更新浏览器
      缺点：
      	1.几乎无法有效对服务端计算机资源文件进行保护
      	2.服务端计算机工作压力异常巨大-----》【B/S通信下高并发解决方案】
      ```

8. 共享资源文件

   1. 什么是共享资源文件

      ​	可以通过网络进行传输的文件，都被称为共享资源文件
      ​	所有的文件内容都可以通过网络传输，所有文件都是共享资源文件

   2. Http服务器下对于共享资源文件分类

      - 静态资源文件
      - 动态资源文件

   3. 静态资源文件

      - 如果文件内容是固定，这种文件可以被称为【静态资源文件】 (文档，图片，视频)
      - 如果文件存放不是内容而是命令，这些命令只能在浏览器编译与执行这种文件可以被称为【静态资源文件】
        （.html，.css，.js）

   4. 动态资源文件

      - 如果文件存放命令，并且命令不能在浏览器编译与执行；只能在服务端计算机编译执行，这样的文件可以被称为【动态资源文件】（.class）

   5. 静态资源文件与动态资源文件调用区别

      ```java
      静态文件被索要时，Http服务器直接通过【输出流】将静态文件中内容或则命令
      以【二进制形式】推送给发起请求浏览器
      
      动态文件被索要时，Http服务器需要创建当前class文件的实例对象,通过实例对象
      调用对应的方法处理用户请求，通过【输出流】将运行结果以【二进制形式】推送
      给发起请求浏览器 
      
          class Student{
              public int add(int num1, int num2){
                  return num1 + num2;
              }
          }
      
      	Http服务器（自动）
              Student stu = new Student();
      	    int 结果 = stu.add(10, 30);
      	    out.print(结果);
      ```
      
      ![image-20211128164403004](00互联网通信流程.assets\image-20211128164403004.png)
   
9. 开发人员在互联网通信流程担负职责

   - 控制浏览器行为 
   - 开发动态资源文件来解决用户需求

![image-20211128170428276](00互联网通信流程.assets\image-20211128170428276.png)

# HTML

### 1.系统结构

```
B/S架构：（以后主要走的方向是这个。）
	Browser/Server （浏览器/服务器的交互形式。)
	Browser(浏览器)支持哪些语言：HTML CSS JavaScript
	写HTML CSS JavaScript代码的这波人职位叫做：WEB前端开发工程师。（Java程序员目前来看也需要会一些前端的东西。）
	前端页面上的图片需要UI设计师完成。（PS对java程序员来说没有太高的要求。）
	S是服务器端Server，Server端的语言很多：C C++ Java python.....（我们主要是使用Java语言完成服务器端的开发）
	B/S架构的系统有什么优点和缺点？
		优点：升级方便，只升级服务器端代码即可。维护成本低。
		缺点：速度慢、体验不好、界面不炫酷
		
	企业内部的解决方案都是采用B/S架构的系统，因为企业内部办公需要的一些系统
	不需要炫酷，不需要特别好的用户体验，只要能做数据的增删改查即可。并且企业
	内部更注重维护的成本。

	B/S架构的系统有哪些代表？
		京东、百度、天猫....
```

```
C/S架构
	Client/Server     （客户端/服务器端的交互形式。）
	缺点：升级麻烦，维护成本较高。
	优点：速度快，体验好，界面炫酷。（娱乐型的系统多数是C/S架构的。）

	常见的C/S架构的系统：
		QQ、微信、支付宝、魔兽、Dota2....
```

### 2.HTML概述

```
1、什么是HTML？
HTML: Hyper Text Markup Language （超文本标记语言）
	由大量的标签组成，每一个标签都有开始标签和结束标签。
	<标签>
		<标签>
			<标签 属性名="属性值" 属性名="属性值">
			</标签>
		</标签>
	</标签>

	超文本: 流媒体、图片、声音、视频....
```

```
2、怎么开发HTML？
HTML开发的时候使用普通的文本编辑器就行，创建的文件扩展名是.html或者.htm
	HTML也有专业的开发工具，例如：DreamWeaver、HBuilder.....
```

```
3、怎么运行HTML？	
直接采用浏览器打开HTML文件就是运行。
```

```
4、HTML是谁制定的？
W3C：世界万维网联盟
W3C制定了HTML的规范，每个浏览器生产厂家都会遵守规范。HTML程序员也会按照这个规范去写代码。
HTML规范目前最高的版本是：HTML5.0，简称H5.
我们这里学习HTML4.0（主要是学习一下HTML的基础用法。）

W3C制定了很多规范：
	HTML/XML/http协议/https安全协议......

为了方便中国web前端程序员的开发，提供大量的帮助文档。为开发提供方便。
	w3school:先出现的，和W3C没有关系
	w3cschool：后出现的，和W3C没有关系
```

### 3.HTML代码及页面展示

#### 3.1 第一个HTML文件

```html
<!--
	1、这是HTML的注释
	2、加上以下代码的第一行就表示HTML5语法。去掉就表示HTML4.0
	3、HTML不区分大小写，语法松散不严格。
-->
<!DOCTYPE html>
<!--根-->
<html>
	
	<!--头-->
	<head>
		<meta charset="utf-8">
		<!--网页标题，显示在网页左上角-->
		<title>网页的标题</title>
	</head>
	
	<!--体-->
	<body>
		网页的主体内容，欢迎学习HTML！
	</body>
	
</html>
```

![image-20211128175200835](01HTML_CSS_JavaScript.assets\image-20211128175200835.png)

#### 3.2 HTML的基本标签

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML的基本标签</title>
	</head>
	<body>
		<!--段落标记-->
		<p>
			《黛玉葬花》是文学名著《红楼梦》中的经典片段。林黛玉最怜惜花，觉得花落以后埋在土里最干净，说明
			她对美有独特的见解。她写了葬花词，以花比喻自己，在《红楼梦》中是最美丽的诗歌之一。贾宝玉和林黛
			玉在葬花的时候有一段对话，成为《红楼梦》中一场情人之间解除误会的绝唱。
		</p>
		<p>
			《黛玉葬花》是文学名著《红楼梦》中的经典片段。林黛玉最怜惜花，觉得花落以后埋在土里最干净，说明
			 她对美有独特的见解。她写了葬花词，以花比喻自己，在《红楼梦》中是最美丽的诗歌之一。贾宝玉和林黛
			 玉在葬花的时候有一段对话，成为《红楼梦》中一场情人之间解除误会的绝唱。
		</p>
		
		<!--标题字：是HTML预留的格式，和word中的标题字相同-->
		<h1>一级标题</h1>
		<h2>二级标题</h2>
		<h3>三级标题</h3>
		<h4>四级标题</h4>
		<h5>五级标题</h5>
		<h6>六级标题</h6>
		
		<!--换行标记，br标签是一个独目标记-->
		hello 
		world!
		hello <br>world!
		
		<!--水平线，独目标记-->
		<hr>
		<!--color和width都是hr标签的属性-->
		<hr color="red" width="50%">
		<!--语法太松散了。-->
		<hr color='green' width=30%>
		
		<!--预留格式 把格式留住-->
		<pre>
		for(int i = 0; i < 10; i++){
			System.out.println("i = " + i);
		}
		</pre>
		
		<del>删除字</del>
		<ins>插入字</ins>
		<b>粗体字</b>
		<i>斜体字</i>
		
		10<sup>2</sup><!--右上角加字-->
		
		10<sub>m</sub><!--右下角加字-->
		
		<!--font标签-->
		<font color="red" size="50">字体标签</font>
	</body>
</html>
```

![image-20211128180538937](01HTML_CSS_JavaScript.assets\image-20211128180538937.png)

#### 3.3 HTML的实体符号

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>实体符号</title>
	</head>
	<body>
		b<a>c
		
		<!-- 实体符号特点是：以‘&’开始，以‘;’结束。'&lt;' 是小于号,'&gt;'是大于号 -->
		b&lt;a&gt;c
		
		<!--加空格   &nbsp; 是空格-->
		abc                                                        def<br>
		a&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bc
	</body>
</html>
```

![image-20211128181104971](01HTML_CSS_JavaScript.assets\image-20211128181104971.png)

#### 3.4 HTML的表格*

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>表格</title>
	</head>
	<body>
		<br><br><br><br><br><br><br><br>
		<center><h1>员工信息列表</h1></center>
		<hr>
		<!--
			border="1px" 设置表格的边框为1像素宽度。
			width 宽度
			height 高度
		-->
		<!--
		<table border="1px" width="300px">
		-->
		<!--表格-->
		<table align="center" border="1px" width="60%" height="150px">
			<!--align对齐方式-->
			<tr align="center"><!--行-->
				<td>a</td><!--单元格-->
				<td>b</td>
				<td>c</td>
			</tr>
			<tr>
				<td>d</td>
				<td>e</td>
				<td>f</td>
			</tr>
			<tr>
				<td>x</td>
				<td>y</td>
				<td align="center">z</td>
			</tr>
		</table>
	</body>
</html>
```

![image-20211128181833570](01HTML_CSS_JavaScript.assets\image-20211128181833570.png)

#### 3.5 HTML的单元格合并

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>表格单元格合并，以及th标签</title>
	</head>
	<body>
		
		<!--注意事项：
			1、row合并的时候，删除“下面的”单元格
			2、col合并的时候，对删除哪个没有要求。
		-->
		<table border="1px" width="50%">
			<tr>
				<!--
				<td>员工编号</td>
				<td>员工薪资</td>
				<td>部门名称</td>
				-->
				<!-- th 标签也是单元格标签，比td多的是居中、加粗。-->
				<th>员工编号</th>
				<th>员工薪资</th>
				<th>部门名称</th>
			</tr>
			<tr>
				<td>1</td>
				<td>2</td>
				<td>3</td>
			</tr>
			<tr>
				<td>a</td>
				<td>b</td>
				<td rowspan="2">f</td><!--row合并-->
			</tr>
			<tr>
				<td colspan="2">d</td><!--col合并-->
				<!--
				<td>f</td>
				-->
			</tr>
		</table>
	</body>
</html>
```

![image-20211128182507931](01HTML_CSS_JavaScript.assets\image-20211128182507931.png)

#### 3.6 thead、tbody、tfoot

```html
<!DOCTYPE html>
<html>
	<head>
		<!--这行代码的作用是告诉浏览器采用哪一种字符集打开当前页面。
		注意：并不是设置当前页面的字符编码方式。-->
		<meta charset="gbk">
		<title>thead、tbody、tfoot在table中不是必须的，只是这样做便于后期的JS代码的编写。</title>
	</head>
	<body>
		<table border="1px" width="50%">
			<!--头-->
			<thead>
				<tr>
					<th>员工编号</th>
					<th>员工薪资</th>
					<th>部门名称</th>
				</tr>
			</thead>
		
			<!--体-->
			<tbody>
				<tr>
					<td>1</td>
					<td>2</td>
					<td>3</td>
				</tr>
				<tr>
					<td>a</td>
					<td>b</td>
					<td rowspan="2">f</td>
				</tr>
				<tr>
					<td colspan="2">d</td>
				</tr>
			</tbody>
		
			<!--脚-->
			<tfoot>
				<tr>
					<td>1</td>
					<td>2</td>
					<td>3</td>
				</tr>
			</tfoot>
		</table>
        
	</body>
</html>
```

![image-20211128183114418](01HTML_CSS_JavaScript.assets\image-20211128183114418.png)

#### 3.7 背景颜色和背景图片

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>背景颜色和背景图片</title>
	</head>
	<!--
		bgcolor : 设置背景色
		background : 设置背景图片
		以上的设置都是对背景进行设置。
		背景图片会覆盖背景色
	-->
	<body bgcolor="red" background="img/bd_logo1.png">
	</body>
</html>
```

![image-20211128183926210](01HTML_CSS_JavaScript.assets\image-20211128183926210.png)

#### 3.8 HTML图片img标签

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>图片img标签</title>
	</head>
	<body>
		<!--
			1、设置图片宽度和高度的时候，只设置宽度，高度会进行等比例缩放。
			2、img标签就是图片标签
			3、src属性是图片的路径
			4、width设置宽度,height设置高度
			5、title设置鼠标悬停时显示的信息。
			6、alt设置图片加载失败时显示的提示信息。
		-->
		<img src="img/bd_logo1.png" width="100px" title="我是百度图片哦" alt="图片找不到哦!"/>
		
		<img src="img/bd_logo1.png" width="100px" title="我是百度图片哦" alt="图片找不到哦!"></img>
		
		<br><br><br>
		
		<img src="img/bd_logo1.png" />
	</body>
</html>
```

![image-20211128184425765](01HTML_CSS_JavaScript.assets\image-20211128184425765.png)

#### 3.9 HTML超链接、热链接*

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>超链接 热链接</title>
	</head>
	<body>
		<!--
			超链接的特点：
				有下划线
				鼠标停留在超链接上面显示小手形状。
				点击超链接之后还能跳转页面。
		-->
		<a href="http://www.baidu.com">百度</a>
		<a href="http://www.jd.com/">京东商城</a>
		<a href="http://www.tmall.com/">天猫</a>
		<a href="http://www.bilibili.com/">BiliBili</a>
		
		<br><br>
		
		<!--
			href：hot references 热引用
			href属性后面一定是一个资源的地址。
			
			href后面的路径可以是绝对路径也可以是相对路径，可以是网络中某个资源的路径，也可以是本地资源的路径。
		-->
		<a href="07.html">07.html</a>
		
		<!--图片超链接-->
		<a href="https://www.baidu.com/">
			<img src="img/bd_logo1.png" width="120px"/>
		</a>
		
		<!--
			超链接有一个target属性：
				可取值：
					_blank : 新窗口
					_self ： 当前窗口（默认就是这种方式。）
					_top ： 顶级窗口
					_parent ： 父窗口
		-->
		<a href="https://www.hao123.com/" target="_self">
			<img src="img/hao123.png" width="120px"/>
		</a>
	</body>
</html>

<!--
	超链接的作用：
		通过超链接可以从浏览器向服务器发送请求。
		浏览器向服务器发送数据（请求：request）
		服务器向浏览器发送数据（响应：response）
		
		B/S结构的系统：每一个请求都会对应一个响应。
	
	用户点击超链接和用户在浏览器地址栏上直接输入URL，有什么区别？
		本质上没有区别，都是向服务器发送请求。
	
	从操作上来讲，超链接使用更方便。
-->
```

![image-20211128185345888](01HTML_CSS_JavaScript.assets\image-20211128185345888.png)

#### 3.10 HTML列表

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>列表</title>
	</head>
	<body>
		<!--有序列表-->
		<ol type="I">
			<li>水果
				<ol type="a">
					<li>苹果</li>
					<li>香蕉</li>
					<li>橘子</li>
				</ol>
			</li>
			<li>蔬菜
				<ol type="a">
					<li>西红柿</li>
					<li>黄瓜</li>
				</ol>
			</li>
			<li>甜点</li>
		</ol>
		
		<hr>
		
		<!--无序列表-->
		<ul type="circle">
			<li>中国
				<ul type="square">
					<li>南京
						<ul type="disc">
							<li>浦口区</li>
							<li>玄武区</li>
							<li>栖霞区</li>
						</ul>
					</li>		
					<li>盐城</li>		
					<li>无锡</li>		
				</ul>	
			</li>
			<li>美国</li>
			<li>日本</li>
		</ul>
		
	</body>
</html>
```

![image-20211128190918494](01HTML_CSS_JavaScript.assets\image-20211128190918494.png)

#### 3.11 HTML表单*

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>表单form</title>
	</head>
	<body>
		<!--
			1、表单有什么用？
				收集用户信息。表单展现之后，用户填写表单，点击提交按钮提交数据给服务器。
			2、怎么画一个表单？
				使用form标签画表单。
			3、一个网页当中可以有多个表单form。
			4、表单最终是需要提交数据给服务器的，form标签有一个action属性，这个属性用来指定服务器地址：
				action属性用来指定数据提交给哪个服务器。
				action属性和超链接中的href属性一样。都可以向服务器发送请求（request）
			5、http://192.168.111.3:8080/oa/save 这是请求路径，表单提交数据最终提交给：
				192.168.111.3机器上的8080端口对应的软件。
		-->
		
		<form action="http://192.168.111.3:8080/oa/save">
			<!-- 画一个提交按钮，这个按钮可以提交表单-->
			<!-- 画按钮可以使用input输入域，type="submit"的时候表示该按钮是一个提交按钮，具有提交表单的能力。-->
			<!-- 对于按钮来说，按钮的value属性用来指定按钮上显示的文本信息。-->
			<input type="submit" value="登录"/>提交表单的按钮<br>
			文本框<input type="text" value="文本框"/><br>
			密码框<input type="password" value="密码框"/><br>
			<input type="checkbox"/>复选框<br>
			<input type="radio"/>单选按钮<br>
			
			<!--这是一个普通按钮，不具备提交表单的能力。-->
			<input type="button" value="设置按钮上显示的文本"/>
		</form>
		
		<!--超链接-->
		<a href="http://www.bilibili.com" target="_blank">哔哩哔哩</a>
		<!--这个按钮和普通的超链接没什么太大的区别。（超链接和表单都可以向服务器发送请求，只不过表单发送请求的同时可以携带数据。）-->
		<form action="http://www.bilibili.com">
			<input type="submit" value="哔哩哔哩" />
		</form>
		
		<hr >
		
		<form action="http://localhost:8080/jd/login">
			用户名<input type="text" /><br>
			密码<input type="password" /><br>
			<input type="submit" value="登录" />
		</form>
		
		<!--
			表单是以什么格式提交数据给服务器的？
				http://localhost:8080/jd/login?username=abc&userpwd=111
				格式：action?name=value&name=value&name=value&name=value&name=value...
				W3C的HTTP协议规定的，必须以这种格式提交给服务器。
			
			重点强调：表单项写了name属性的，一律会提交给服务器。不想提交这一项，就不要写name属性。
			
			文本框和密码框的value不需要程序员指定，用户输入什么value就是什么。
			
			当name没有写的时候，该项不会提交给服务器。
			但是当value没有写的时候，value的默认值是空字符串""，会将空字符串提交给服务器。java代码得到的是：String username = "";
		-->
		<hr >
		<form action="http://localhost:8080/jd/login">
			<table>
				<tr>
					<td>用户名</td>
					<td><input type="text" name="username" /></td>
				</tr>
				<tr>
					<td>密码</td>
					<td><input type="password" name="userpwd" /></td>
				</tr>
				<tr align="center">
					<td colspan="2">
						<input type="submit" value="登录" />
						&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
						<input type="reset" value="清空" />
					</td>
				</tr>
			</table>
		</form> 
		
		<!--submit必须放到form标签内部-->
		<input type="submit" value="登录" />
		<!--必须放到form标签内部-->
		<input type="reset" value="清空" />
		<form></form>
	</body>
</html>
```

![image-20211128194258978](01HTML_CSS_JavaScript.assets\image-20211128194258978.png)

#### 3.12 用户注册的表单

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>用户注册的表单</title>
	</head>
	<body>
		<!--
			用户注册：
				用户名
				姓名
				密码
				确认密码
				性别
				兴趣爱好
				学历
				简介
			
			form表单method属性：
				get:采用get方式提交的时候，用户提交的信息会显示在浏览器的地址栏上。
				post：采用post方式提交的时候，用户提交的信息不会显示在浏览器地址栏上。
				当用户提交的信息中含有敏感信息，例如：密码，建议采用post方式提交。
			
			method属性不指定，或者指定get，这种情况下都是get。
			只有当method属性指定为post的时候才是post请求。
			剩下所有的请求都是get请求。
			
			post提交的时候提交的数据格式和get还是一样的，只不过不再地址栏上显示出来。
			POST提交的数据还是：name=value&name=value&name=value.....
			
			<!--
			get提交
			http://localhost:8080/jd/register?username=qqq&userpwd=123&gender=1&interest=comic&grade=bk&introduce=good+man
			-->
			<!--
			post提交
			http://localhost:8080/jd/register
			-->
		-->
		<form action="http://localhost:8080/jd/register" method="post">
			用户名
			<input type="text" name="username"/>
			<br>
			密码
			<input type="password" name="userpwd"/>
			<br>
			确认密码
			<input type="password"/>
			<br>
			性别<!--单选按钮的value必须手动指定--><!--checked表示默认选中-->
			<input type="radio" name="gender" value="1" checked/>男
			<input type="radio" name="gender" value="0"/>女
			<br>
			兴趣爱好
			<input type="checkbox" name="interest" value="game" />游戏
			<input type="checkbox" name="interest" value="bodybuilding" />健身
			<input type="checkbox" name="interest" value="comic" checked/>动漫
			<br>
			学历
			<select name="grade"><!--下拉按钮-->
				<option value ="gz">高中</option>
				<option value ="dz">大专</option>
				<option value ="bk" selected>本科</option><!--默认选中-->
				<option value ="ss">硕士</option>
			</select>
			<br><br>
			简介<!--文本域，文本域没有value属性，用户填写的内容就是value-->
			<textarea rows="10" cols="60" name="introduce"></textarea>
			<br>
			<input type="submit" value="注册" />
			<input type="reset" value="清空" />
		</form>
		
		<hr >
		<!--超链接也可以提交数据给服务器，但是提交的数据都是固定不变的。-->
		<!--超链接是get请求。不是post请求。-->
		<!--http://localhost:8080/oa/save?username=zhangsan&password=111-->
		<a href="http://localhost:8080/oa/save?username=zhangsan&password=111">提交</a>
	</body>
</html>
```

![image-20211128201054520](01HTML_CSS_JavaScript.assets\image-20211128201054520.png)

#### 3.13 下拉列表支持多选

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>下拉列表支持多选</title>
	</head>
	<body>
		<!-- multiple="multiple" 支持多选的 size设置显示条目数量。-->
		<select multiple="multiple" size="4">
			<option>江苏省</option>
			<option>河南省</option>
			<option>山东省</option>
			<option>山西省</option>
		</select>
	</body>
</html>
```

![image-20211128201415695](01HTML_CSS_JavaScript.assets\image-20211128201415695.png)

#### 3.14 file控件

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>file控件</title>
	</head>
	<body>
		<!--file控件：文件上传专用。-->
		<input type="file" />
		
		<form action="http://localhost:8080/oa/save">
			<!--隐藏域：网页上看不到，但是表单提交的时候数据会自动提交给服务器。-->
			<input type="hidden" name="userid" value="111" />
			
			用户代码<input type="text" name="usercode" />
			<input type="submit" value="提交" />
			<!--http://localhost:8080/oa/save?userid=111&usercode=qqq-->
		</form>
	</body>
</html>
```

![image-20211128202015506](01HTML_CSS_JavaScript.assets\image-20211128202015506.png)

#### 3.15 readonly和disabled

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>readonly disabled</title>
	</head>
	<body>
		<!--
			readonly和disabled相同点：都是只读不能修改。
			但是readonly可以提交给服务器，disabled数据不会提交（即使有name属性也不会提交。）
		-->
		<form action="http://localhost:8080/taobao/save">
			用户代码<input type="text" name="usercode" value="110" readonly />
			<br>
			用户姓名<input type="text" name="username" value="zhangsan" disabled />
			<br>
			<input type="submit" value="提交数据" />
			<!--http://localhost:8080/taobao/save?usercode=110-->
		</form>
	</body>
</html>
```

![image-20211128202950487](01HTML_CSS_JavaScript.assets\image-20211128202950487.png)

#### 3.16 input控件的maxlength属性

```HTML
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>maxlength</title>
	</head>
	<body>
		<!--
			maxlength 设置文本框中可输入的字符数量。
		-->
		<input type="text" maxlength="3" />
	</body>
</html>
```

![image-20211128203407899](01HTML_CSS_JavaScript.assets\image-20211128203407899.png)

#### 3.17 HTML中元素的id属性

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中元素的id属性</title>
	</head>
	<body id="mybody">
		<!--
			1、在HTML文档当中，任何元素（节点）都有id属性，id属性是该节点的唯一标识。所以在同一个HTML文档当中id值不能重复。
			2、注意：表单提交数据的时候，只和name有关系，和id无关。
			3、id有什么用？
				javascript语言：可以对HTML文档当中的任意节点进行增删改操作。
				javascript可以对HTML文档当中的任意节点进行增删改，那么增删改之前需要先拿到这个节点，通常我们通过id来拿节点对象。
				id的存在让我们获取元素（节点）更方便。
			4、HTML文档是一棵树，树上有很多节点，每一个节点都有唯一的id。
				javascript主要就是对这棵DOM树上的节点进行增删改的。
				DOM(Document)树。
		-->
		<form id="myform">
			<input type="text" id="username" name="username"/>
			<input type="password" id="userpwd" name="userpwd"/>
			
			<!--id就是节点的身份证号码，不能重复。-->·
			<!-- <input type="text" id="username" /> -->
		</form>
	</body>
</html>
```

![image-20211128205134153](01HTML_CSS_JavaScript.assets\image-20211128205134153.png)

#### 3.18 HTML中的div和span

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中的div和span</title>
	</head>
	<body>
		<!--
			1、div和span是什么？有什么用？
				* div和span都可以称为“图层”
				* 图层的作用是为了保证页面可以灵活的布局。
				* 图层就是一个一个的盒子，div嵌套div就是盒子套盒子。
				* div和span是可以定位的，只要定下div的左上角的x轴和y轴坐标即可。
			2、其实最早的网页是采用table进行布局的，但是table不灵活，太死板。
			现代的网页开发中div布局使用最多，几乎很少使用table进行布局了。
			
			3、div和span的区别？
				div独自占用一行（默认情况下） 
				span不会独自占用一行。
		-->
		<div id="div1">我是一个DIV</div>
		<div id="div2">我是一个DIV</div>
		
		<span id="span1">我是一个SPAN标签</span>
		<span id="span2">我是一个SPAN标签</span>
		
		<div id="div3">
			<div>
				<div>test</div>
			</div>
		</div>
		
	</body>
</html>
```

![image-20211128205341871](01HTML_CSS_JavaScript.assets\image-20211128205341871.png)

------

# CSS

### 1、CSS概述

```
1、什么是CSS，有什么作用？
	CSS(Cascading Style Sheet):层叠样式表语言。
	CSS的作用是：
		修饰HTML页面，设置HTML页面中的某些元素的样式，让HTML页面更好看。
		CSS好比是HTML的化妆品一样。
	HTML还是主体，CSS依赖HTML。CSS的存在就是修饰HTML，所以新建的文件还是xx.html文件。
```

```
2、CSS我们要求掌握到什么程度？
	* 常见的CSS样式要求会写。
	* 别人写的CSS样式要能看懂。
```

```html
3、在HTML页面中嵌套使用CSS的三种方式：
第一种方式：在标签内部使用style属性来设置元素的CSS样式，这种方式称为内联定义方式。
	语法格式：
		<标签 style="样式名:样式值; 样式名:样式值; 样式名:样式值;..."></标签>
		
第二种方式：在head标签中使用style块，这种方式被称为样式块方式。
	语法格式：
		<head>
			<style type="text/css">
				选择器 {
					样式名 : 样式值;
					样式名 : 样式值;
					.....
				}
				选择器 {
					样式名 : 样式值;
					样式名 : 样式值;
					.....
				}
			</style>
		</head>

第三种方式：链入外部样式表文件，这种方式最常用。（将样式写到一个独立的xxx.css文件当中，在需要的网页上
直接引入css文件，样式就引入了）
	语法格式：
		<link type="text/css" rel="stylesheet" href="css文件的路径" />
	这种方式易维护，维护成本较低。
		xxx.css文件
			1.html中引入了
			2.html中引入了
			3.html中引入了
			4.html中引入了
```

```html
4、HTML标签属性分类
1.基本属性：
	大多数HTML标签都拥有属性，是一个非常庞大群体
	比如 id属性，相当于身份证编号，用于区分HTML标签
	<input type="text" id="one"/>
	<input type="text" id="two"/>
	比如 name属性，相当于人名字,允许一组标签拥有相同name
	<input type="text" id="one"  name="myText"/>
	<input type="text" id="two"  name="myText"/>

2.样式属性：
    是一个非常庞大群体，通知浏览器将HTML标签中数据在
	浏览器中以指定形态展示
	<div style="background-color:red;color:green;width:300px;height:200px"></div>

3.工作状态属性:
	只存在于【表单域标签】中，用于表示【表单域标签】状态.
		 checked:存在于radio与checkbox中，表示标签是否被选中
		 disabled:表示标签处于不可用状态
		 readOnly:表示标签处于只读状态
		 seleteced：存在option标签，表示标签是否被选中

4.监听属性：
	监听属性用户与HTML标签之间进行通信通道，监听属性
	用于监听用户在何时对当前标签进行何种操作,当指定
	操作产生时，监听属性将会通知浏览器调用对应JavaScript
	方法处理当前请求
```

```html
5、CSS选择器
1.介绍：
    CSS选择器，实际上就是一组定位条件用于定位HTML标签。
	CSS选择器有9个大的分类
2.CSS选择器语法格式:
      <html>
		 <head>
			<!--type='text/css'，-->
			<style type="text/css">
				定位条件{
					"样式属性1":"值1";
					"样式属性2"："值2"
				}
			</style>
		</head>
	</html>
```

```html
6、id选择器
1.介绍：
	根据HTML标签中ID属性的值进行定位
2.语法:
    <style type="text/css">
         #id编号{
		    "样式属性1":"值1";
			"样式属性2"："值2"
		}
	</style>
```

```html
7、标签选择器
1.介绍:
	根据HTML标签类型进行定位
2.语法:
	<style type="text/css">
		标签类型名{
		    "样式属性1":"值1";
			"样式属性2"："值2"
		}
	</style>
```

```html
8、层级选择器
1.HTML标签之间关系：
	父子关系    兄弟关系
2.父子关系:
	即为包含关系
		<tr>
			<td>
				<input type="text">
			</td>
		</tr>
		td标签是tr标签的子标签
		input标签是td标签的子标签
3.兄弟关系:
	一组标签拥有相同的父标签，并且彼此之间
	没有任何包含关系，即为兄弟
		<body>
			<div>1</div>   大哥
			<p>2</p>       二哥
			<span>3</span> 三弟
		</body>
4.层级选择器介绍:
	根据标签之间父子关系或则兄弟关系进行定位
5.简单的层级选择器                
     <style type="text/css">
        定位父标签条件  定位子标签条件{
			      
		}
        找到指定父标签下满足条件的所有子标签
	</style>
```

```html
9、自定义选择器
1.介绍：
	如果一组HTML标签之间没有相同的特征，但是却需要
	对指定属性赋值相同内容，此时将自定义选择器绑定
	到对应标签上
2.语法:
	<style type="text/css">
         .自定义选择器名{
			 color:red;
		}
	</style>

	<div class="自定义选择器名"></div>
	<p   class="自定义选择器名"></p>
```

### 2、CSS代码及展示

#### 2.1 内联定义方式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中引入CSS样式的第一种方式：内联定义方式</title>
	</head>
	<body>
		<!--
			width 宽度样式
			height 高度样式
			background-color 背景色样式
			display 布局样式（none表示隐藏，block表示显示）
			border-style 边框样式
			border-width 边框宽度
			border-color 边框颜色
		-->
		<div style="width:300px; height:300px; background-color:#CCFFFF; display:block;
		border-color:red; border-width:1px; border-style:solid;"></div>
		
		<br><br>
		
		<!--
			样式还能这样写：
				border : 1px solid black;
		-->
		<div style="width:300px; height:300px; background-color:#CCFFFF; display:block;
		border:1px solid black;"></div>
	</body>
</html>
```

<img src="01HTML_CSS_JavaScript.assets\image-20211204144951032.png" alt="image-20211204144951032" style="zoom: 67%;" />

#### 2.2 样式块

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中引入CSS样式的第二种方式：样式块</title>
		
		<style type="text/css">
			/*  这是CSS的注释。 */
			/*
				id选择器
				语法格式：
					#id{
						样式名 : 样式值;
						样式名 : 样式值;
						....
					}
			*/
		   #usernameErrorMsg {
		   	    color : red;
		   	    font-size : 12px;
		   }
		   
		   /*
		   	标签选择器
		   	语法格式：
		   		标签名 {
		   			样式名 : 样式值;
		   			样式名 : 样式值;
		   			样式名 : 样式值;
		   			....
		   		}
		   	标签选择器作用的范围比id选择器广。
		   */
		   div {
		   	background-color : black;
		   	border : 1px solid red;
		   	width : 100px;
		   	height : 50px;
		   }
		   
		   /*
		   	类选择器
		   	语法格式：
		   		.类名{
		   			样式名 : 样式值;
		   			样式名 : 样式值;
		   			样式名 : 样式值;
		   			....
		   		}
		   */
		   .student {
		   	border : 1px solid red;
		   	width : 400px;
		   	height : 30px;
		   }
		</style>
	</head>
	<body>
		<!--
			设置样式字体大小12px，颜色为红色！
		-->
		<span id="usernameErrorMsg">对不起，用户名不能为空！</span>
		
		<div></div>
		<div></div>
		
		<!--class相同的标签可以认为是同一类标签。-->
		<br><br><br>
		<input type="text" class="student"/>
		
		<br><br><br>
		
		<select class="student">
			<option>研究生</option>
			<option>本科</option>
		</select>
	</body>
</html>
```

![image-20211204150453349](01HTML_CSS_JavaScript.assets\image-20211204150453349.png)

#### 2.3 引入外部独立的css文件

```css
/* 路径：css/1.css */
a{
	text-decoration: none; /*下划线消失*/
}
/*
	cursor : 鼠标样式，pointer是小手，hand也是，但是hand有浏览器兼容问题。建议使用pointer
*/
#baiduSpan {
	text-decoration: underline; /*下划线出现*/
	cursor: pointer;
}
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>在HTML中使用CSS样式的第三种方式：引入外部独立的css文件</title>
		<!--引入css-->
		<link rel="stylesheet" type="text/css" href="css/1.css" />
	</head>
	<body>
		<a href="http://www.baidu.com">百度</a>
		<span id="baiduSpan">点击我链接到百度哦！</span>
	</body>
</html>
```

![image-20211204151856109](01HTML_CSS_JavaScript.assets\image-20211204151856109.png)

#### 2.4 列表样式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>列表样式</title>
		<style type="text/css">
			ul{
				/*list-style-type: none;*/
				/*list-style-type: circle ;*/
				list-style-type: square ;
			}
		</style>
	</head>
	<body>
		<ul>
			<li>中国
				<ul>
					<li>北京</li>
					<li>上海</li>
					<li>重庆</li>
				</ul>
			</li>
			<li>美国</li>
			<li>俄国</li>
		</ul>
	</body>
</html>
```

![image-20211204152255461](01HTML_CSS_JavaScript.assets\image-20211204152255461.png)

#### 2.5 CSS样式的绝对定位

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>CSS样式的绝对定位</title>
		<style type="text/css">
			#div1{
				background-color: red;
				border: 1px black solid;
				width: 300px;
				height: 300px;
				position : absolute; /*绝对定位*/
				left: 100px; /*离左边100像素*/
				top: 100px; /*离顶部100像素*/
			}
		</style>
	</head>
	<body>
		<div id="div1"></div>		
	</body>
</html>
```

![image-20211204152712149](01HTML_CSS_JavaScript.assets\image-20211204152712149.png)

------

# JavaScript

### 1、JavaScript概述

```
1、什么是JavaScript，有什么用？
	JavaScript是运行在浏览器上的脚本语言。简称JS。
	JavaScript是网景公司（NetScape）的 布兰登艾奇（JavaScript之父）开发的，最初叫做LiveScript。
	LiveScript的出现让浏览器更加的生动了，不再是单纯的静态页面了。页面更具有交互性。
	在历史的某个阶段，SUN公司和网景公司他们之间有合作关系，SUN公司把LiveScript的名字修改为JavaScript。
	JavaScript这个名字中虽然带有“Java”但是和Java没有任何关系，只是语法上有点类似。他们运行的位置不同，
	Java运行在JVM当中，JavaScript运行在浏览器的内存当中。

	JavaScript程序不需要我们程序员手动编译，编写完源代码之后，浏览器直接打开解释执行。
	JavaScript的“目标程序”以普通文本形式保存，这种语言都叫做“脚本语言”。

	Java的目标程序以.class形式存在，不能使用文本编辑器打开，不是脚本语言。

	网景公司1998年被美国在线收购。

	网景公司最著名的就是领航者浏览器：Navigator浏览器。

	LiveScript的出现，最初的时候是为Navigator浏览器量身定制一门语言，不支持其他浏览器。
	
	当Navigator浏览器使用非常广泛的时候，微软害怕了，于是微软在最短的时间内组建了一个团队，
	开始研发只支持IE浏览器的脚本语言，叫做JScript。

	JavaScript和JScript并存的年代，程序员是很痛苦的，因为程序员要写两套程序。
	在这种情况下，有一个非营利性组织站出来了，叫做ECMA组织（欧洲计算机协会）
	ECMA根据JavaScript制定了ECMA-262号标准，叫做ECMA-Script。

	现代的javascript和jscript都实现了ECMA-Script规范。（javascript和jscript统一了。）

	以后大家会学习一个叫做JSP的技术，JSP和JS有啥区别？
		JSP : JavaServer Pages（隶属于Java语言的，运行在JVM当中）
		JS : JavaScript（运行在浏览器上。）
```

```
2、在HTML中怎么嵌入JavaScript代码？
	三种方式。
```

### 2、JavaScript语言代码

#### 2.1 ECMAScript

##### 2.1.1 HTML中嵌入JS代码的第一种方式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中嵌入JS代码的第一种方式</title>
	</head>
	<body>
		<!--
			1、要实现的功能：
				用户点击以下按钮，弹出消息框。
				
			2、JS是一门事件驱动型的编程语言，依靠事件去驱动，然后执行对应的程序。
			在JS中有很多事件，其中有一个事件叫做：鼠标单击，单词：click。并且任何
			事件都会对应一个事件句柄叫做：onclick。
			【注意：事件和事件句柄的区别是：事件句柄是在事件单词前添加一个on。】，
			而事件句柄是以HTML标签的属性存在的。
		
			3、onclick="js代码"，执行原理是什么？
				页面打开的时候，js代码并不会执行，只是把这段JS代码注册到按钮的click事件上了。
				等这个按钮发生click事件之后，注册在onclick后面的js代码会被浏览器自动调用。
			
			4、怎么使用JS代码弹出消息框？
				在JS中有一个内置的对象叫做window，全部小写，可以直接拿来使用，window代表的是浏览器对象。
				window对象有一个函数叫做:alert，用法是：window.alert("消息");这样就可以弹窗了。
			
			5、JS中的字符串可以使用双引号，也可以使用单引号。
		
			6、JS中的一条语句结束之后可以使用分号“;”，也可以不用。
		-->
		<input type="button" value="hello" onclick="window.alert('hello js')"/>
		
		<input type="button" value="hello" onclick='window.alert("hello jscode")'/>
		
		<input type="button" value="hello" onclick="window.alert('hello zhangsan')
												window.alert('hello lis')
												window.alert('hello wangwu')"/>
		
		<!-- window. 可以省略。-->
		<input type="button" value="hello" onclick="alert('hello zhangsan')
												alert('hello lis')
												alert('hello wangwu')"/>
	</body>
</html>
```

![image-20211204160942872](01HTML_CSS_JavaScript.assets\image-20211204160942872.png)

##### 2.1.2 HTML中嵌入JS代码的第二种方式：脚本块

```html
<!--
	javascript的脚本块在一个页面当中可以出现多次。没有要求。
	javascript的脚本块出现位置也没有要求，随意。
-->
<script type="text/javascript">
	// alert有阻塞当前页面加载的作用。（阻挡，直到用户点击确定按钮。）
	window.alert("first.......");
</script>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中嵌入JS代码的第二种方式</title>
		<!--样式块-->
		<style type="text/css">
			/*
				css代码
			*/
		</style>
		
		<!--脚本块-->
		<script type="text/javascript">
			window.alert("head............");
		</script>
	</head>
	<body>
		<input type="button" value="我是一个按钮对象1" />
		
		<!--第二种方式：脚本块的方式-->
		<script type="text/javascript">
			/*
				暴露在脚本块当中的程序，在页面打开的时候执行，
				并且遵守自上而下的顺序依次逐行执行。（这个代
				码的执行不需要事件）
			*/
			window.alert("Hello World!"); // alert函数会阻塞整个HTML页面的加载。
			
			// JS代码的注释，这是单行注释。
			/*
				JS代码的多行注释。和java一样。
			*/
			window.alert("Hello JavaScript!");
		</script>
		
		<input type="button" value="我是一个按钮对象" />
	</body>
</html>

<script type="text/javascript">
window.alert("last.......");
</script>
```

##### 2.1.3 引入外部独立的js文件

```javascript
//路径  js/1.js
window.alert("hello js!");
window.alert("hello js!");
window.alert("hello js!");
window.alert("hello js!");
window.alert("hello js!");
window.alert("hello js test!");
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HTML中嵌入JS代码的第三种方式：引入外部独立的js文件。</title>
	</head>
	<body>
		<!--在需要的位置引入js脚本文件-->
		<!--引入外部独立的js文件的时候，js文件中的代码会遵循自上而下的顺序依次逐行执行。-->
		<script type="text/javascript" src="js/1.js"></script>
		
		<!--同一个js文件可以被引入多次。但实际开发中这种需求很少。-->
		<script type="text/javascript" src="js/1.js"></script>
		
		<!--这种方式不行，结束的script标签必须有。-->
		<!--
		<script type="text/javascript" src="js/1.js" />
		-->
		<!--
		<script type="text/javascript" src="js/1.js"></script>
		-->
		
		<script type="text/javascript" src="js/1.js">
			// 这里写的代码不会执行。
			// window.alert("Test");
		</script>
		
		<script type="text/javascript">
			alert("hello jack!");
		</script>
	</body>
</html>
```

##### 2.1.4 关于JS中的变量

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>关于JS中的变量</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
				回顾java中的变量：
					1、java中怎么定义/声明变量？
						数据类型 变量名;
						例如：
							int i;
							double d;
							boolean flag;
					2、java中的变量怎么赋值？
						使用“=”运算符进行赋值运算。（"="运算符右边先执行，将右边执行的结果赋值给左边的变量。）
						变量名 = 值;
						例如：
							i = 10;
							d = 3.14;
							flag = false;
					3、java语言是一种强类型语言，强类型怎么理解？
						java语言存在编译阶段，假设有代码：int i;
						那么在Java中有一个特点是：java程序编译阶段就已经确定了
						i变量的数据类型，该i变量的数据类型在编译阶段是int类型，
						那么这个变量到最终内存释放，一直都是int类型，不可能变成
						其他类型。
							int i = 10;
							double d = i; 
							这行代码是说声明一个新的变量d，double类型，把i变量中保存的值传给d。
							i还是int类型。
							
							i = "abc"; 这行代码编译的时候会报错，因为i变量的数据类型是int类型，不能将字符串赋给i。
							
							java中要求变量声明的时候是什么类型，以后永远都是这种类型，不可变。编译期强行固定变量的数据类型。
							称为强类型语言。
							
							public void sum(int a, int b){}
							sum(?,?);
							
				javascript当中的变量？
					怎么声明变量？
						var 变量名;
					怎么给变量赋值？
						变量名 = 值;
					javascript是一种弱类型语言，没有编译阶段，一个变量可以随意赋值，赋什么类型的值都行。
						var i = 100;
						i = false;
						i = "abc";
						i = new Object();
						i = 3.14;
					重点：JavaScript是一种弱类型编程语言。
			*/
		   // 在JS当中,当一个变量没有手动赋值的时候,系统默认赋值undefined
		   var i;
		   // undefined 在JS中是一个具体存在值.
		   alert("i = " + i);  //i = undefined
		   
		   alert(undefined); // undefined
		   var k = undefined;
		   alert("k = " + k);// k = undefined
		   
		   // 一个变量没有声明/定义,直接访问?
		   // alert(age); //语法错误：age is not defined (变量age不存在。不能这样写。)
		   
		   var a, b, c = 200;
		   alert("a = " + a);// a = undefined
		   alert("b = " + b);// b = undefined
		   alert("c = " + c);// c = 200
		   
		   a = false;
		   alert(a); // false
		   
		   a = "abc";
		   alert(a); // abc
		   
		   a = 1.2;
		   alert(a); // 1.2
		</script>
	</body>
</html>
```

##### 2.1.5 JS函数初步

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS函数初步</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
			1、JS中的函数：
				等同于java语言中的方法，函数也是一段可以被重复利用的代码片段。
				函数一般都是可以完成某个特定功能的。
					
			2、回顾java中的方法？
				[修饰符列表] 返回值类型 方法名(形式参数列表){
					方法体;
				}
				例如：
				public static boolean login(String username,String password){
					...
					return true;
				}
				boolean loginSuccess = login("admin","123");
					
			3、JS中的变量是一种弱类型的，那么函数应该怎么定义呢？
				语法格式：
					第一种方式：
						function 函数名(形式参数列表){
							函数体;
						}
					第二种方式：
						函数名 = function(形式参数列表){
							函数体;
						}	
					重点：JS中的函数不需要指定返回值类型，返回什么类型都行。
			*/
		   function sum(a, b){
		   		// a和b都是局部变量,他们都是形参(a和b都是变量名，变量名随意。)
		   		alert(a + b);
		   }
		   // 函数必须调用才能执行的.
		   // sum(10, 20);//30
		   
		   // 定义函数sayHello
		   sayHello = function(username){
		   		alert("hello " + username);
		   }
		   // 调用函数
		   //sayHello("zhangsan");//hello zhangsan
		</script>
		
		<input type="button" value="hello" onclick="sayHello('jack');" />
		<input type="button" value="计算10和20的求和" onclick="sum(10, 20);" />
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS函数初步</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
			java中的方法有重载机制，JS中的函数能重载吗？
			JS当中的函数在调用的时候，参数的类型没有限制，并且参数的个数也没有限制，JS就是这么随意。（弱类型）
							
			重载的含义：
			方法名或者函数名一样，形参不同（个数、类型、顺序）
			*/
			function sum(a, b){
				return a + b;
			}
			
			// 调用函数sum
			var retValue = sum(1, 2);
			alert(retValue);//3
			
			var retValue2 = sum("jack"); // jack赋值给a变量,b变量没有赋值系统默认赋值undefined
			alert(retValue2); // jackundefined //jack + undefined
			
			var retValue3 = sum();
			alert(retValue3); // NaN (NaN是一个具体存在的值，该值表示不是数字。Not a Number)
			
			var retValue4 = sum(1, 2, 3);
			alert("结果=" + retValue4); // 结果=3
			
			function test1(username){
				alert("test1");
			}
			//在JS当中，函数的名字不能重名，当函数重名的时候，后声明的函数会将之前声明的同名函数覆盖。
			function test1(){
				alert("test1 test1");
			}
			
			test1("lisi"); // test1 test1 // 这个调用的是第二个test1()函数.
		</script>
	</body>
</html>
```

##### 2.1.6 JS的局部变量和全局变量

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS的局部变量和全局变量</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
			全局变量：
				在函数体之外声明的变量属于全局变量，全局变量的生命周期是：
					浏览器打开时声明，浏览器关闭时销毁，尽量少用。因为全局变量会一直在浏览器的内存当中，耗费内存空间。
					能使用局部变量尽量使用局部变量。
			局部变量：
				在函数体当中声明的变量，包括一个函数的形参都属于局部变量，
				局部变量的生命周期是：函数开始执行时局部变量的内存空间开辟，函数执行结束之后，局部变量的内存空间释放。
				局部变量生命周期较短。
			*/
		   // 全局变量
		   var i = 100;
		   function accessI(){
		   		// 访问的是全局变量
		   		alert("i = " + i);
		   }
		   accessI();//i = 100
		   
		   // 全局变量
		   var username = "jack";
		   function accessUsername(){
		   		// 局部变量
		   		var username = "lisi";
		   		// 就近原则:访问局部变量
		   		alert("username = " + username); // username = lisi
		   }
		   // 调用函数
		   accessUsername();
		   // 访问全局变量
		   alert("username = " + username); // username = jack
		   
		   function accessAge(){
		   		var age = 20;
		   		alert("年龄 = " + age);
		   }
		   accessAge(); //年龄 = 20
		   // 报错(语法不对)
		   // alert("age = " + age);
		   
		   // 以下语法是很奇怪的.
		   function myfun(){
		   		// 当一个变量声明的时候没有使用var关键字,那么不管这个变量是在哪里声明的,都是全局变量.
		   		myname = "dujubin";
		   }
		   // 访问函数
		   myfun(); 
		   alert("myname = " + myname); // myname = dujubin
		</script>
	</body>
</html>
```

##### 2.1.7 JS中的数据类型

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS中的数据类型</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
			1、虽然JS中的变量在声明的时候不需要指定数据类型，但是在赋值，每一个数据还是有类型的，所以
			这里也需要学习一下JS包括哪些数据类型？
				JS中数据类型有：原始类型、引用类型。
				原始类型：Undefined、Number、String、Boolean、Null
				引用类型：Object以及Object的子类
			2、ES规范(ECMAScript规范)，在ES6之后，又基于以上的6种类型之外添加了一种新的类型：Symbol
			3、JS中有一个运算符叫做typeof，这个运算符可以在程序的运行阶段动态的获取变量的数据类型。
				typeof运算符的语法格式：
					typeof 变量名
				typeof运算符的运算结果是以下6个字符串之一：注意字符串都是全部小写。
					"undefined"
					"number"
					"string"
					"boolean"
					"object"
					"function"
			4、在JS当中比较字符串是否相等使用“==”完成。没有equals。
			*/
		   // 求和,要求a变量和b变量将来的数据类型必须是数字,不能是其他类型
		   // 因为以下定义的这个sum函数是为了完成两个数字的求和.
		   function sum(a, b){
		   		if(typeof a == "number" && typeof b == "number"){
		   			return a + b;
		   		}
		   		alert(a + "," + b + "必须都为数字！");
		   }
		   // 别人去调用以上你写的sum函数.
		   var retValue = sum(false, "abc");// false,abc必须都为数字！
		   alert(retValue); // undefined
		   var retValue2 = sum(1, 2);
		   alert(retValue2); // 3
		   
		   var i;
		   alert(typeof i); // undefined
		   
		   var k = 10;
		   alert(typeof k); // number
		   
		   var f = "abc";
		   alert(typeof f); // string
		   
		   var d = null;
		   alert(typeof d); // object  //null属于Null类型,但是typeof运算符的结果是"object"
		   
		   var flag = false;
		   alert(typeof flag); // boolean
		   
		   var obj = new Object();
		   alert(typeof obj); // object
		   
		   // sayHello是一个函数.
		   function sayHello(){}
		   alert(typeof sayHello); // function
		</script>
	</body>
</html>
```

##### 2.1.8 Undefined类型

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Undefined类型</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
				Undefined类型只有一个值，这个值就是 undefined
				当一个变量没有手动赋值，系统默认赋值undefined
				或者也可以给一个变量手动赋值undefined。
			*/
			var i; // undefined
			var k = undefined; // undefined
			
			alert(i == k); // true
			
			var y = "undefined"; // "undefined" 字符串
			alert(y == k); // false
		</script>
	</body>
</html>
```

##### 2.1.9 Number类型

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Number类型</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
			1、Number类型包括哪些值？
				-1 0 1 2 2.3 3.14 100 .... NaN Infinity
				整数、小数、正数、负数、不是数字、无穷大都属于Number类型。
			2、isNaN() : 结果是true表示不是一个数字，结果是false表示是一个数字。
			3、parseInt()函数: 可以将字符串自动转换成数字,并且取整数位.
			4、parseFloat()函数: 可以将字符串自动转换成数字.
			5、Math.ceil() 函数（Math是数学类，数学类当中有一个函数叫做ceil()，作用是向上取整。）
		*/	
			var v1 = 1;
			var v2 = 3.14;
			var v3 = -100;
			var v4 = NaN; //不是一个数字
			var v5 = Infinity;//无穷大
			
			alert(typeof v1);// "number"
			alert(typeof v2);// "number"
			alert(typeof v3);// "number"
			alert(typeof v4);// "number"
			alert(typeof v5);// "number"
			
			// 关于NaN (表示Not a Number，不是一个数字，但属于Number类型)
			// 什么情况下结果是一个NaN呢？
			// 运算结果本来应该是一个数字,最后算完不是一个数字的时候,结果是NaN.
			var a = 100;
			var b = "中国人";
			alert(a / b); // 除号显然最后结果应该是一个数字,但是运算的过程中导致最后不是一个数字,那么最后的结果是NaN
			
			var e = "abc";
			var f = 10;
			alert(e + f); // "abc10"
			
			// Infinity (当除数为0的时候，结果为无穷大)
			alert(10 / 0);
			
			// 思考:在JS中10 / 3 = ?
			alert(10 / 3); // 3.3333333333333335
			
			// 关于isNaN函数？
			// 用法：isNaN(数据) ,结果是true表示不是一个数字, 结果是false表示是一个数字.
			// isNaN : is Not a Number 
			function sum(a, b){
				if(isNaN(a) || isNaN(b)){
					alert("参与运算的必须是数字！");
					return;
				}
				return a + b;
			}
			sum(100, "abc"); // 参与运算的必须是数字！
			alert(sum(100, 200)); // 300
			
			// parseInt():可以将字符串自动转换成数字,并且取整数位.
			alert(parseInt("3.9999")); // 3
			alert(parseInt(3.9999)); // 3
			
			// parseFloat():可以将字符串自动转换成数字.
			alert(parseFloat("3.14") + 1); // 4.140000000000001
			alert(parseFloat("3.2") + 1); // 4.2
			
			// Math.ceil() 向上取整
			alert(Math.ceil("2.1")); // 3
		</script>
	</body>
</html>
```

##### 2.1.10 Boolean类型

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Boolean类型</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
			1、 JS中的布尔类型永远都只有两个值：true和false （这一点和java相同。）
			2、在Boolean类型中有一个函数叫做：Boolean()。
				语法格式：
					Boolean(数据) 
				Boolean()函数的作用是将非布尔类型转换成布尔类型。
		*/
			var username = "lucy";
			//var username = "";
			if(Boolean(username)){ //为 "lucy" 进去，为""进else
				alert("欢迎你" + username);
			}else{
				alert("用户名不能为空！");
			}

			if(username){ //为 "lucy" 进去，为""进else
				alert("欢迎你" + username);
			}else{
				alert("用户名不能为空！");
			}
			
			// 规律:“有"就转换成true,"没有"就转换成false.
			alert(Boolean(1));         // true
			alert(Boolean(0));         // false
			alert(Boolean(""));        // false
			alert(Boolean("abc"));     // true
			alert(Boolean(null));      // false
			alert(Boolean(NaN));       // false
			alert(Boolean(undefined)); // false
			alert(Boolean(Infinity));  // true
			 
			/*while(10 / 3){ // 死循环
				alert("hehe");
			}*/
			 
			 for(var i = 0; i < 10; i++){
				alert("i = " + i);
			 }
			 // Null类型只有一个值,null
			alert(typeof null); // "object"
		</script>
	</body>
</html>
```

##### 2.1.11 String类型

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>String类型</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
		String类型：
			1、在JS当中字符串可以使用单引号，也可以使用双引号。
				var s1 = 'abcdef';
				var s2 = "test";
			2、在JS当中，怎么创建字符串对象呢？
				两种方式：
					第一种：var s = "abc";
					第二种（使用JS内置的支持类String）：var s2 = new String("abc");
				需要注意的是：String是一个内置的类，可以直接用，String的父类是Object。
			3、无论小string还是大String，他们的属性和函数都是通用的。
			4、关于String类型的常用属性和函数？
				常用属性：
					length 获取字符串长度
				常用函数：
					indexOf			获取指定字符串在当前字符串中第一次出现处的索引
					lastIndexOf		获取指定字符串在当前字符串中最后一次出现处的索引
					replace			替换
					substr			截取子字符串
					substring		截取子字符串
					toLowerCase		转换小写
					toUpperCase		转换大写
					split			拆分字符串
		*/
			// 小string(属于原始类型String)
			var x = "king";
			alert(typeof x); // "string"
			
			// 大String(属于Object类型)
			var y = new String("abc");
			alert(typeof y); // "object"
			
			// 获取字符串的长度
			alert(x.length); // 4
			alert(y.length); // 3
			
			alert("http://www.baidu.com".indexOf("http")); // 0
			alert("http://www.baidu.com".indexOf("https")); // -1
			
			// 判断一个字符串中是否包含某个子字符串？
			alert("http://www.baidu.com".indexOf("https") >= 0 ? "包含" : "不包含"); // 不包含
			
			// replace (注意：只替换了第一个)
			alert("name=value%name=value%name=value".replace("%","&")); // name=value&name=value%name=value
			
			// 继续调用replace方法,就会替换第“二”个.
			// 想全部替换需要使用正则表达式.
			alert("name=value%name=value%name=value".replace("%","&").replace("%", "&")); // name=value&name=value&name=value
			
			// 考点:经常问 substr和substring的区别？
			// substr(startIndex, length)
			alert("abcdefxyz".substr(2,4)); //cdef
			// substring(startIndex, endIndex) 注意:含头不含尾
			alert("abcdefxyz".substring(2,4)); //cd
		</script>
	</body>
</html>
```

##### 2.1.12 Object类型

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Object类型</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
			Object类型：
				1、Object类型是所有类型的超类，自定义的任何类型，默认继承Object。
				2、Object类包括哪些属性？
					prototype属性（常用的，主要是这个）：作用是给“类”动态的扩展属性和函数。
					constructor属性
				3、Object类包括哪些函数？
					toString()
					valueOf()
					toLocaleString()
				4、在JS当中定义的类默认继承Object，会继承Object类中所有的属性以及函数。
				换句话说，自己定义的类中也有prototype属性。
				5、在JS当中怎么定义类？怎么new对象？
					定义类的语法：
						第一种方式：
							function 类名(形参){	
							}
						第二种方式：
							类名 = function(形参){	
							}
					创建对象的语法：
						new 构造方法名(实参); // 构造方法名和类名一致。
		*/
			function sayHello(){		   
			}
			// 把sayHello当做一个普通的函数来调用.
			sayHello();
			// 这种方式就表示把sayHello当做一个类来创建对象.
			var obj = new sayHello(); // obj是一个引用,保存内存地址指向堆中的对象.
			 
			// 定义一个学生类
			function Student(){
				alert("Student.....");
			}
			// 当做普通函数调用
			Student(); // Student.....
			// 当做类来创建对象
			var stu = new Student(); // Student.....
			alert(stu); // [object Object]
			 
			// JS中的类的定义,同时又是一个构造函数的定义
			// 在JS中类的定义和构造函数的定义是放在一起来完成的.
			function User(a, b, c){ // a b c是形参,属于局部变量.
				// 声明属性 (this表示当前对象)
				// User类中有三个属性:sno/sname/sage
				this.sno = a;
				this.sname = b;
				this.sage = c;
			}
			// 创建对象
			var u1 = new User(111, "zhangsan", 30);
			// 访问对象的属性
			alert(u1.sno);   // 111
			alert(u1.sname); // zhangsan
			alert(u1.sage);  // 30
			
			var u2 = new User(222, "jackson", 55);
			alert(u2.sno);   // 222
			alert(u2.sname); // jackson
			alert(u2.sage);  // 55 
			// 访问一个对象的属性,还可以使用这种语法
			alert(u2["sno"]);   // 222
			alert(u2["sname"]); // jackson
			alert(u2["sage"]);  // 55
			 
			// 定义类的另一种语法			
			Emp = function(ename,sal){
				// 属性
				this.ename = ename;
				this.sal = sal;
			}
			var e1 = new Emp("SMITH", 800);
			alert(e1["ename"] + "," + e1.sal); // SMITH,800
			
			Product = function(pno,pname,price){
				// 属性
				this.pno = pno;
				this.pname = pname;
				this.price = price;
				// 函数
				this.getPrice = function(){
					return this.price;
				}
			 }
			 var xigua = new Product(111, "西瓜", 4.0);
			 var pri = xigua.getPrice();
			 alert(pri); // 4.0
			 
			 // 可以通过prototype这个属性来给类动态扩展属性以及函数
			 Product.prototype.getPname = function(){
				return this.pname;
			 }
			 
			 // 调用后期扩展的getPname()函数
			 var pname = xigua.getPname();
			 alert(pname) // 西瓜
			
			// 给String扩展一个函数
			 String.prototype.suiyi = function(){
				alert("这是给String类型扩展的一个函数，叫做suiyi");
			 }
			 "abc".suiyi(); // 这是给String类型扩展的一个函数，叫做suiyi
		</script>
	</body>
</html>

<!--
	java语言怎么定义类，怎么创建对象？（强类型）
		public class User{
			private String username;
			private String password;
			public User(){
			}
			public User(String username,String password){
				this.username = username;
				this.password = password;
			}
		}
		User user = new User();
		User user = new User("lisi","123");
		
	JS语言怎么定义类，怎么创建对象？（弱类型）
		User = function(username,password){
			this.username = username;
			this.password = password;
		}
		var u = new User();
		var u = new User("zhangsan");
		var u = new User("zhangsan","123");
-->
```

##### 2.1.13 null、NaN、undefined的区别

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>null NaN undefined这三个值有什么区别</title>
	</head>
	<body>
		<script type="text/javascript">
			// null NaN undefined 数据类型不一致.
			alert(typeof null); // object
			alert(typeof NaN); // number
			alert(typeof undefined); // undefined
			
			// null和undefined可以等同.
			alert(null == NaN); // false
			alert(null == undefined); // true
			alert(undefined == NaN); // false
			
			// 在JS当中有两个比较特殊的运算符
			// ==(等同运算符：只判断"值"是否相等)
			// ===(全等运算符：既判断"值"是否相等，又判断"数据类型"是否相等)
			alert(null === NaN); // false
			alert(null === undefined); // false
			alert(undefined === NaN); // false
        
			alert(1 == true); // true 
			alert(1 === true); // false
		</script>
	</body>
</html>
```

##### 2.1.14 JS的常用事件-注册事件的两种方式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS的常用事件-注册事件的两种方式</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
			JS中的事件：
				blur      失去焦点	
				focus     获得焦点
				
				click     鼠标单击
				dblclick  鼠标双击
				
				keydown   键盘按下
				keyup     键盘弹起
				
				mousedown 鼠标按下
				mouseover 鼠标经过
				mousemove 鼠标移动
				mouseout  鼠标离开
				mouseup   鼠标弹起
				
				reset     表单重置
				submit    表单提交
				
				change    下拉列表选中项改变，或文本框内容改变
				select    文本被选定
				load      页面加载完毕（整个HTML页面中所有的元素全部加载完毕之后发生。）
			
			任何一个事件都会对应一个事件句柄，事件句柄是在事件前添加on。
			onXXX这个事件句柄出现在一个标签的属性位置上。（事件句柄以属性的形式存在。）
		*/
	   
		// 对于当前程序来说,sayHello函数被称为回调函数(callback函数)
		// 回调函数的特点:自己把这个函数代码写出来了,但是这个函数不是自己负责调用,由其他程序负责调用该函数.
			function sayHello(){
				alert("hello js!");
			}	
		</script>
		
		<!--注册事件的第一种方式，直接在标签中使用事件句柄-->
		<!--以下代码的含义是：将sayHello函数注册到按钮上，等待click事件发生之后，该函数被浏览器调用。我们称这个函数为回调函数。-->
		<input type="button" value="hello" onclick="sayHello()"/>
		
		<input type="button" value="hello2" id="mybtn" />
		<input type="button" value="hello3" id="mybtn1" />
		<input type="button" value="hello4" id="mybtn2" />
		<script type="text/javascript">
			function doSome(){
				alert("do some!");
			}
			/*
				第二种注册事件的方式，是使用纯JS代码完成事件的注册。
			*/
		   // 第一步:先获取这个按钮对象(document是全部小写，内置对象，可以直接用，document就代表整个HTML页面)
		   var btnObj = document.getElementById("mybtn");
		   // 第二步:给按钮对象的onclick属性赋值
		   btnObj.onclick = doSome; // 注意:千万别加小括号. btnObj.onclick = doSome();这是错误的写法.
									// 这行代码的含义是,将回调函数doSome注册到click事件上.
		   
		   var mybtn1 = document.getElementById("mybtn1");
		   mybtn1.onclick = function(){ // 这个函数没有名字,叫做匿名函数,这个匿名函数也是一个回调函数.
			   alert("test.........."); // 这个函数在页面打开的时候只是注册上,不会被调用,在click事件发生之后才会调用.
		   }
		   
		   document.getElementById("mybtn2").onclick = function(){
			   alert("test22222222.........");
		   }
		</script>
		
	</body>
</html>

<!--
java中也有回调函数机制：
	public class MyClass{
		public static void main(String[] args){
			// 主动调用run()方法，站在这个角度看run()方法叫做正向调用。
			run();
		}
			
		// 站在run方法的编写者角度来看这个方法，把run方法叫做回调函数。
		public static void run(){
			System.out.println("run...");
		}
	}
-->
```

##### 2.1.15 JS代码的执行顺序

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<!-- load事件什么时候发生？页面全部元素加载完毕之后才会发生。-->
	<body onload="ready()">
		<script type="text/javascript">
			/*
			// 第一步:根据id获取节点对象
			var btn = document.getElementById("btn"); // 返回null(因为代码执行到此处的时候id="btn"的元素还没有加载到内存)
			// 第二步:给节点对象绑定事件
			btn.onclick = function(){
				alert("hello js");
			}
			*/
		   function ready(){
			   var btn = document.getElementById("btn");
			   btn.onclick = function(){
			   	alert("hello js");
			   }
		   }
		</script>
		<input type="button" value="hello" id="btn" />
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS代码的执行顺序</title>
	</head>
	<body>
		<script type="text/javascript">
			// 页面加载的过程中,将a函数注册给了load事件
			// 页面加载完毕之后,load事件发生了,此时执行回调函数a
			// 回调函数a执行的过程中,把b函数注册给了id="btn"的click事件
			// 当id="btn"的节点发生click事件之后,b函数被调用并执行.
			window.onload = function(){ // 这个回调函数叫做a
				document.getElementById("btn").onclick = function(){ // 这个回调函数叫做b
					alert("hello js..........");
				}
			}
		</script>
		
		<input type="button" value="hello" id="btn" />
	</body>
</html>
```

##### 2.1.16 JS代码设置节点的属性

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS代码设置节点的属性</title>
	</head>
	<body>
		<script type="text/javascript">
			window.onload = function(){
				document.getElementById("btn").onclick = function(){
					var mytext = document.getElementById("mytext");
					// 一个节点对象中只要有的属性都可以"."
					if(mytext.type == "text"){
						mytext.type = "checkbox";
					}else if(mytext.type == "checkbox"){
						mytext.type = "text";
					}
				}
			}
		</script>
		<input type="text" id="mytext"/>
		<input type="button" value="将文本框修改为复选框" id="btn"/>
	</body>
</html>
```

##### 2.1.17 JS代码捕捉回车键

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS代码捕捉回车键</title>
	</head>
	<body>
		<script type="text/javascript">
			window.onload = function(){
				var usernameElt = document.getElementById("username");
				// 回车键的键值是13
				// ESC键的键值是27
				/*
				usernameElt.onkeydown = function(a, b, c){
					// 获取键值
					// alert(a); // [object KeyboardEvent]
					// alert(b);
					// alert(c);
				}
				*/
				/*
				usernameElt.onkeydown = function(event){
					// 获取键值
					// 对于“键盘事件对象"来说,都有keyCode属性用来获取键值.
					alert(event.keyCode);
				}
				*/
				usernameElt.onkeydown = function(event){
					if(event.keyCode === 13){
						alert("正在进行验证....");
					}
				}		  
			}
		</script>
		
		<input type="text" id="username"/>
	</body>
</html>
```

##### 2.1.18 JS的void运算符

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS的void运算符</title>
	</head>
	<body>
		<!--
			void运算符的语法：void(表达式)
			运算原理：执行表达式，但不返回任何结果。
				javascript:void(0)
				其中javascript:作用是告诉浏览器后面是一段JS代码。
				以下程序的javascript:是不能省略的。
		-->
		<a href="javascript:void(0)" onclick="window.alert('test code')">
			既保留住超链接的样式，同时用户点击该超链接的时候执行一段JS代码，但页面还不能跳转。
		</a>
		
		<br>
		
		<a href="javascript:void(100)" onclick="window.alert('test code')">
			既保留住超链接的样式，同时用户点击该超链接的时候执行一段JS代码，但页面还不能跳转。
		</a>
		
		<br>
		
		<!--void() 这个小括号当中必须有表达式-->
		<a href="javascript:void()" onclick="window.alert('test code')">
			既保留住超链接的样式，同时用户点击该超链接的时候执行一段JS代码，但页面还不能跳转。
		</a>
		
		<br><br><br>
	</body>
</html>
```

##### 2.1.19 JS的控制语句

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS的控制语句</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
			1、if
			2、switch
			
			3、while
			4、do .. while..
			5、for循环
			
			6、break
			7、continue
			
			8、for..in语句（了解）
			9、with语句（了解）
		*/
			// 创建JS数组
			var arr = [false,true,1,2,"abc",3.14]; // JS中数组中元素的类型随意.元素的个数随意.
			// 遍历数组
			for(var i = 0; i < arr.length; i++){
				alert(arr[i]);
			}
			// for..in
			for(var i in arr){
				//alert(i);//下标
				alert(arr[i]);
			}
			
			// for..in语句可以遍历对象的属性
			User = function(username,password){
				this.username = username;
				this.password = password;
			}
			
			var u = new User("张三", "444");
			alert(u.username + "," + u.password);//张三,444
			alert(u["username"] + "," + u["password"]);//张三,444
			for(var shuXingMing in u){
				//alert(shuXingMing)
				//alert(typeof shuXingMing) // shuXingMing是一个字符串
				alert(u[shuXingMing]);//属性名
			}
			
			alert(u.username);//张三
			alert(u.password);//444
			
			with(u){
				alert(username + "," + password);//张三,444
			}
		</script>
	</body>
</html>

<!--
public class Test{
	public static void main(String[] args){
		int[] arr = {1,2,3,4,5,6};
		int[] arr2 = new int[5]; // 等同于：int[] arr2 = {0,0,0,0,0};
		String[] arr3 = {"a","b","c"};
		String[] arr4 = new String[3]; // 等同于：String[] arr4 = {null,null,null};
	}
}
-->
```

#### 2.2 DOM编程

##### 2.2.1 获取文本框的value

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>DOM编程-获取文本框的value</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
				1、JavaScript包括三大块：
					ECMAScript：JS的核心语法（ES规范 / ECMA-262标准）
					DOM：Document Object Model（文档对象模型：对网页当中的节点进行增删改的过程。）HTML文档被当做一棵DOM树来看待。
						var domObj = document.getElementById("id");
					BOM：Browser Object Model（浏览器对象模型）
						关闭浏览器窗口、打开一个新的浏览器窗口、后退、前进、浏览器地址栏上的地址等，都是BOM编程。
				2、DOM和BOM的区别和联系？
					BOM的顶级对象是：window
					DOM的顶级对象是：document
					实际上BOM是包括DOM的！
			*/
		   /*
		   window.onload = function(){
			   //var btnElt = window.document.getElementById("btn");
			   var btnElt = document.getElementById("btn");
			   alert(btnElt); // object HTMLInputElement
		   }
		   */
		  
		  window.onload = function(){
			  var btnElt = document.getElementById("btn");
			  btnElt.onclick = function(){
				  // 获取username节点
				  var usernameElt = document.getElementById("username");
				  var username = usernameElt.value;
				  alert(username);
				  
				 // alert(document.getElementById("username").value);
				 
				 // 可以修改它的value
				 document.getElementById("username").value = "zhangsan";
			  }
		  }
		</script>
		<input type="text" id="username" />
		<input type="button" value="获取文本框的value" id="btn"/>
		<!--
		<input type="button" id="btn" value="hello" />
		-->
	
	
		<hr>
		<script type="text/javascript">
			window.onload = function(){
				document.getElementById("setBtn").onclick = function(){
					document.getElementById("username2").value = document.getElementById("username1").value;
				}
			}
		</script>
		<input type="text" id="username1" />
		<br>
		<input type="text" id="username2" />
		<br>
		<input type="button" value="将第一个文本框中的value赋值到第二个文本框上" id="setBtn" />
		
		<hr >
		<!--blur事件：失去焦点事件-->
		<!--以下代码中的this代表的是当前input节点对象,this.value就是这个节点对象的value属性。-->
		<input type="text" onblur="alert(this.value)" />
	</body>
</html>
```

##### 2.2.2 innerHTML和innerText操作div和span

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>DOM编程-innerHTML和innerText操作div和span</title>
		<style type="text/css">
			#div1{
				background-color: aquamarine;
				width: 300px;
				height: 300px;
				border: 1px black solid;
				position: absolute;
				top: 100px;
				left: 100px;
			}
		</style>
	</head>
	<body>
		<!--
			innerText和innerHTML属性有什么区别？
				相同点：都是设置元素内部的内容。
				不同点：
					innerHTML会把后面的“字符串”当做一段HTML代码解释并执行。
					innerText，即使后面是一段HTML代码，也只是将其当做普通的字符串来看待。
		-->
		<script type="text/javascript">
			window.onload = function(){
				var btn = document.getElementById("btn");
				btn.onclick = function(){
					// 设置div的内容
					// 第一步:获取div对象
					var divElt = document.getElementById("div1");
					// 第二步:使用innerHTML属性来设置元素内部的内容
					// divElt.innerHTML = "fjdkslajfkdlsajkfldsjaklfds";
					divElt.innerHTML = "<font color='red'>用户名不能为空！</font>";//用户名不能为空！
					// divElt.innerText = "<font color='red'>用户名不能为空！</font>";//<font color='red'>用户名不能为空！</font>
				}
			}
		</script>
		
		<input type="button" value="设置div中的内容" id="btn"/>
		<div id="div1"></div>
	</body>
</html>
```

##### 2.2.3 正则表达式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>DOM编程-关于正则表达式</title>
	</head>
	<body>
		<script type="text/javascript">
		/*
			1、什么是正则表达式，有什么用？
				正则表达式：Regular Expression
				正则表达式主要用在字符串格式匹配方面。
			2、正则表达式实际上是一门独立的学科，在Java语言中支持，C语言中也支持，javascript中也支持。
			大部分编程语言都支持正则表达式。正则表达式最初使用在医学方面，用来表示神经符号等。目前使用最多
			的是计算机编程领域，用作字符串格式匹配。包括搜索方面等。
			3、正则表达式，对于我们javascript编程来说，掌握哪些内容呢？
				第一：常见的正则表达式符号要认识。
				第二：简单的正则表达式要会写。
				第三: 他人编写的正则表达式要能看懂。
				第四：在javascript当中，怎么创建正则表达式对象！（new对象）
				第五：在javascript当中，正则表达式对象有哪些方法！（调方法）
				第六：要能够快速的从网络上找到自己需要的正则表达式。并且测试其有效性。
			4、常见的正则表达式符号？
				.   匹配除换行符以外的任意字符 
				\w  匹配字母或数字或下划线或汉字 
				\s  匹配任意的空白符 
				\d  匹配数字 
				\b  匹配单词的开始或结束 
				^   匹配字符串的开始 
				$   匹配字符串的结束
				 
				*     重复零次或更多次 
				+     重复一次或更多次 
				?     重复零次或一次 
				{n}   重复n次 
				{n,}  重复n次或更多次 
				{n,m} 重复n到m次
				 
				\W       匹配任意不是字母，数字，下划线，汉字的字符 
				\S       匹配任意不是空白符的字符 
				\D       匹配任意非数字的字符 
				\B       匹配不是单词开头或结束的位置 
				[^x]     匹配除了x以外的任意字符 
				[^aeiou] 匹配除了aeiou这几个字母以外的任意字符 
				
				正则表达式当中的小括号()优先级较高。
				[1-9]        表示1到9的任意1个数字（次数是1次。）
				[A-Za-z0-9]  表示A-Za-z0-9中的任意1个字符
				[A-Za-z0-9-] 表示A-Z、a-z、0-9、- ，以上所有字符中的任意1个字符。
				
				|    表示或者
			5、简单的正则表达式要会写
				QQ号的正则表达式：^[1-9][0-9]{4,}$
			6、他人编写的正则表达式要能看懂？
				email正则：^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
			7、怎么创建正则表达式对象，怎么调用正则表达式对象的方法？
				第一种创建方式：
					var regExp = /正则表达式/flags;
				第二种创建方式:使用内置支持类RegExp
					var regExp = new RegExp("正则表达式","flags");
					
				关于flags：
					g：全局匹配
					i：忽略大小写
					m：多行搜索（ES规范制定之后才支持m。）当前面是正则表达式的时候，m不能用。只有前面是普通字符串的时候，m才可以使用。
					
				正则表达式对象的test()方法？
					true / false = 正则表达式对象.test(用户填写的字符串);
					true : 字符串格式匹配成功
					false: 字符串格式匹配失败
		*/
			window.onload = function(){
				// 给按钮绑定click
				document.getElementById("btn").onclick = function(){
					var email = document.getElementById("email").value;
					var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
					var ok = emailRegExp.test(email);
					if(ok){
						//合法
						document.getElementById("emailError").innerText = "邮箱地址合法";
					}else{
						// 不合法
						document.getElementById("emailError").innerText = "邮箱地址不合法";
					}
				}
				// 给文本框绑定focus
				document.getElementById("email").onfocus = function(){
					document.getElementById("emailError").innerText = "";
				}
			}
		</script>
		
		<input type="text" id="email" />
		<span id="emailError" style="color: red; font-size: 12px;"></span>
		<br>
		<input type="button" value="验证邮箱" id="btn" />
	</body>
</html>
```

##### 2.2.4 扩展字符串的trim()

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>扩展字符串的trim()</title>
	</head>
	<body>
		<script type="text/javascript">
			// 低版本的IE浏览器不支持字符串的trim()函数,怎么办？
			// 可以自己对String类扩展一个全新的trim()函数!
			String.prototype.trim = function(){
				// alert("扩展之后的trim方法");
				// 去除当前字符串的前后空白
				// 在当前的方法中的this代表的就是当前字符串.
				//return this.replace(/^\s+/, "").replace(/\s+$/, "");
				return this.replace(/^\s+|\s+$/g, "");
			}  
			
			window.onload = function(){
				document.getElementById("btn").onclick = function(){
					// 获取用户名
					var username = document.getElementById("username").value;
					// 去除前后空白
					username = username.trim();
					// 测试
					alert("--->" + username + "<----");
				}
			}
		</script>
		<input type="text" id="username" />
		<input type="button" value="获取用户名" id="btn" />
	</body>
</html>
```

##### 2.2.5 表单验证

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>表单验证</title>
		<style type="text/css">
			span{
				color: red;
				font-size: 12px;
			}
		</style>
	</head>
	<body>
	<!--
	表单验证：
		（1）用户名不能为空
		（2）用户名必须在6-14位之间
		（3）用户名只能有数字和字母组成，不能含有其它符号（正则表达式）
		（4）密码和确认密码一致，邮箱地址合法。
		（5）统一失去焦点验证
		（6）错误提示信息统一在span标签中提示，并且要求字体12号，红色。
		（7）文本框再次获得焦点后，清空错误提示信息，如果文本框中数据不合法要求清空文本框的value
		（8）最终表单中所有项均合法方可提交
	-->
		<script type="text/javascript">
			window.onload = function(){
				//获取用户名以及用户名的span标签
				var usernameElt = document.getElementById("username");
				var usernameErrorSpan = document.getElementById("username_error");
				//给username这个文本框绑定失去焦点blur事件
				usernameElt.onblur = function(){
					//获取用户名
					var username = usernameElt.value;
					//去除前后空白
					username = username.trim();
					//判断用户名是否为空
					if(username === ""){
						//用户名为空
						usernameErrorSpan.innerText = "用户名不能为空";
					}else{
						//用户名不为空
						//继续判断长度[6-14]
						if(username.length < 6 || username.length > 14){
							//用户名长度非法、
							usernameErrorSpan.innerText = "用户长度必须在[6-14]之间";
						}else{
							//用户名长度合法
							//必须判断是否含有特殊符号
							var regExp = /^[A-Za-z0-9]+$/;
							if(!regExp.test(username)){
								//用户名中含有特殊符号
								usernameErrorSpan.innerText = "用户名只能由数字和字母组成";
							}else{
								//用户名最终合法
								
							}
						}
					}
				}
				//给username这个文本框绑定获得焦点focus事件
				usernameElt.onfocus = function(){
					if(usernameErrorSpan.innerText != ""){					
						//清除用户名文本框不合法的value
						usernameElt.value = "";
					}
					//清空usernameErrorspan
					usernameErrorSpan.innerText = "";
				}
				
				//获取确认密码框对象
				var userpwd2Elt = document.getElementById("userpwd2");
				//获取密码错误提示的span标签
				var userpwdErrorSpan = document.getElementById("pwd_error");
				//绑定blur事件
				userpwd2Elt.onblur = function(){
					//获取密码和确认密码
					var userpwdElt = document.getElementById("userpwd");
					var userpwd = userpwdElt.value;
					var userpwd2 = userpwd2Elt.value;
					if(userpwd != userpwd2){
						//密码不一致
						userpwdErrorSpan.innerText = "密码不一致";
					}else{
						//密码一致
					}
				}
				//绑定focus事件
				userpwd2Elt.onfocus = function(){
					if(userpwdErrorSpan.innerText != ""){
						userpwd2Elt.value = "";
					}
					userpwdErrorSpan.innerText = "";
				}
				
				//给email绑定blur事件
				var emailElt = document.getElementById("email");
				//获取email的span
				var emailErrorSpan = document.getElementById("email_error");
				emailElt.onblur = function(){
					//获取email
					var email = emailElt.value;
					//编写email的正则表达式
					var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
					if(!emailRegExp.test(email)){
						//email不合法
						emailErrorSpan.innerText = "邮箱地址不合法";
					}else{
						//email合法
					}
				}
				//给email绑定focus事件
				emailElt.onfocus = function(){
					if(emailErrorSpan.innerText != ""){
						emailElt.value = "";
					}
					emailErrorSpan.innerText = "";
				}
				
				//给提交按钮绑定鼠标单击事件
				var submitBtnElt = document.getElementById("submitBtn");
				submitBtnElt.onclick = function(){
					// 触发username的blur、userpwd2的blur、email的blur
					// 不需要人工操作,使用纯JS代码触发事件.
					usernameElt.focus();
					usernameElt.blur();
					
					userpwd2Elt.focus();
					userpwd2Elt.blur();
					
					emailElt.focus();
					emailElt.blur();
					
					//当所有表单项都是合法的时候，提交表单
					if(usernameErrorSpan.innerText == "" && userpwdErrorSpan.innerText == "" && emailErrorSpan.innerText == ""){
						//获取表单对象
						var userFormElt = document.getElementById("userform");
						//可以在这里设置action，也可以不在这里设置action
						userFormElt.action = "http://localhost:8080/jd/save";
						//提交表单
						userFormElt.submit();
					}
				}
				
			}
		</script>
		
		<!--这个表单提交应该使用post，这里为了检测，所以使用get。-->
		<!-- <form id="userForm" action="http://localhost:8080/jd/save" method="get"> -->
		<form id="userform" method="get">
			用户名<input type="text" name="username" id="username" />
			<span id="username_error"></span>
			<br>
			
			密码<input type="password" name="userpwd" id="userpwd"/><br>
			确认密码<input type="password" id="userpwd2" />
			<span id="pwd_error"></span>
			<br>
			
			邮箱<input type="text" name="email" id="email" />
			<span id="email_error"></span>
			<br><br>
			
			<!-- <input type="submit" value="注册" /> -->
			<input type="button" value="注册" id="submitBtn"/> &nbsp;&nbsp;&nbsp;
			<input type="reset" src="重置" />
		</form>
	</body>
</html>
```

##### 2.2.6 复选框的全选和取消全选

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>复选框的全选和取消全选</title>
	</head>
	<body>
		<script type="text/javascript">
			window.onload = function(){
				var aihaos = document.getElementsByName("aihao");
				var firstChk = document.getElementById("firstChk");
				
				firstChk.onclick = function(){
					for(var i = 0; i < aihaos.length; i++){
						aihaos[i].checked = firstChk.checked;
					}
				}
				
				// 对以上数组进行遍历
				var all = aihaos.length;
				for(var i = 0; i < aihaos.length; i++){
					aihaos[i].onclick = function(){
						var checkedCount = 0;
						// 总数量和选中的数量相等的时候,第一个复选框选中.
						for(var i = 0; i < aihaos.length; i++){
							if(aihaos[i].checked){
								checkedCount++;
							}
						}
						firstChk.checked = (all == checkedCount);
						/*
						if(all == checkedCount){
							firstChk.checked = true;
						}else{
							firstChk.checked = false;
						}
						*/
					}
				}
			}
		</script>
		<input type="checkbox" id="firstChk"/><Br>
		<input type="checkbox" name="aihao" value="movie" />电影<Br>
		<input type="checkbox" name="aihao" value="comic" />动漫<Br>
		<input type="checkbox" name="aihao" value="tv" />剧集<Br>
	</body>
</html>
```

##### 2.2.7 获取下拉列表选中项的value

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>获取下拉列表选中项的value</title>
	</head>
	<body>
		<script type="text/javascript">
			window.onload = function(){
				var provinceListElt = document.getElementById("provinceList");
				provinceListElt.onchange = function(){
					// 获取选中项的values
					alert(provinceListElt.value);
				}
			}
		</script>
		<select id="provinceList">
			<option value ="">--请选择省份--</option>
			<option value ="001">河北省</option>
			<option value ="002">河南省</option>
			<option value ="003">山东省</option>
			<option value ="004">山西省</option>
		</select>
	</body>
</html>

<!--
省份和市区的关系是：1对多
省份表t_province
id      pcode      pname
----------------------------
1       001        河北省
2       002        河南省
3       003        山东省
4       004        山西省

市区表t_city
id      ccode      cname       pcode(fk)
----------------------------------------------
1       101        石家庄         001
2       102        保定           001
3       103        邢台           001
4       104        承德           001
5       105        张家口         001
6       106        邯郸           001
7       107        衡水           001

前端用户选择的假设是河北省，那么必须获取到河北省的pcode，获取到001
然后将001发送提交给服务器，服务器底层执行一条SQL语句：
	select * from t_city where pcode = '001';
	
	返回一个List集合，List<City> cityList;
	
	cityList响应浏览器，浏览器在解析cityList集合转换成一个新的下拉列表。
-->
```

##### 2.2.8 显示网页时钟

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>显示网页时钟</title>
	</head>
	<body>
		<script type="text/javascript">
			/* 关于JS中内置的支持类：Date，可以用来获取时间/日期。*/
			// 获取系统当前时间
			var nowTime = new Date();
			// 输出
			document.write(nowTime);//Fri Dec 10 2021 19:43:35 GMT+0800 (中国标准时间)
			
			// 转换成具有本地语言环境的日期格式.
			nowTime = nowTime.toLocaleString();
			document.write(nowTime);//2021/12/10 下午7:43:35
			document.write("<br>");
			document.write("<br>");
			
			// 当以上格式不是自己想要的,可以通过日期获取年月日等信息,自定制日期格式.
			var t = new Date();
			var year = t.getFullYear(); // 返回年信息,以全格式返回.
			var month = t.getMonth(); // 月份是:0-11
			// var dayOfWeek = t.getDay(); // 获取的一周的第几天(0-6)
			var day = t.getDate(); // 获取日信息.
			document.write(year + "年" + (month+1) + "月" + day + "日");//2021年12月10日
			document.write("<br>");
			document.write("<br>");
			
			// 重点:怎么获取毫秒数？(从1970年1月1日 00:00:00 000到当前系统时间的总毫秒数)
			//var times = t.getTime();
			//document.write(times); // 一般会使用毫秒数当做时间戳. (timestamp)
			document.write(new Date().getTime());//1639136615761
		</script>
		
		<script type="text/javascript">
			function displayTime(){
				var time = new Date();
				time = time.toLocaleString();
				document.getElementById("timeDiv").innerHTML = time;
			}
			
			// 每隔1秒调用displayTime()函数
			function start(){
				// 从这行代码执行结束开始,则会不间断的,每隔1000毫秒调用一次displayTime()函数.
				v = window.setInterval("displayTime()", 1000);	
			}
			function stop(){
				window.clearInterval(v);
				document.getElementById("timeDiv").innerText = "";
			}
		</script>
		<br><br>
		<input type="button" value="显示系统时间" onclick="start();"/>
		<input type="button" value="系统时间停止" onclick="stop();" />
		<div id="timeDiv"></div>
	</body>
</html>
```

##### 2.2.9 内置支持类Array

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>内置支持类Array</title>
	</head>
	<body>
		<script type="text/javascript">
			// 创建长度为0的数组
			var arr = [];
			alert(arr.length);//0
			
			// 数据类型随意
			var arr2 = [1,2,3,false,"abc",3.14];
			alert(arr2.length);//6
			
			// 下标会越界吗
			// arr2 = [1,2,3,false,"abc",3.14,undefined,"test"]
			arr2[7] = "test"; // 自动扩容.
			
			document.write("<br>");
			// 遍历
			for(var i = 0; i < arr2.length; i++){
				document.write(arr2[i] + "<br>");
			}
			
			// 另一种创建数组的对象的方式
			var a = new Array();
			alert(a.length); // 0
			
			var a2 = new Array(3); // 3表示长度.
			alert(a2.length); // 3
			
			var a3 = new Array(3,2);//多个参数表示存入数组的元素
			alert(a3.length); // 2
		   
		    var a = [1,2,3,9];
		    var str = a.join("-");
		    alert(str); // 1-2-3-9
		   
		    // 在数组的末尾添加一个元素(数组长度+1)
		    a.push(10);
		    alert(a.join("-")); // 1-2-3-9-10
		   
		    // 将数组末尾的元素弹出(数组长度-1)
		    var endElt = a.pop();
		    alert(endElt);//10
		    alert(a.join("-"));//1-2-3-9
		   
		    // 注意:JS中的数组可以自动模拟栈数据结构:后进先出,先进后出原则.
		    // push压栈
		    // pop弹栈
		   
		    // 反转数组.
		    a.reverse();
		    alert(a.join("="));// 9=3=2=1
		</script>
	</body>
</html>
```

#### 2.3 BOM编程 

#####  2.3.1 open和close

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>BOM编程-open和close</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
				1、BOM编程中，window对象是顶级对象，代表浏览器窗口。
				2、window有open和close方法，可以开启窗口和关闭窗口。
			*/
		</script>
		
		<input type="button" value="开启百度(新窗口)" onclick="window.open('http://www.baidu.com');" />
		<input type="button" value="开启百度(当前窗口)" onclick="window.open('http://www.baidu.com', '_self');" />
		<input type="button" value="开启百度(新窗口)" onclick="window.open('http://www.baidu.com', '_blank');" />
		<input type="button" value="开启百度(父窗口)" onclick="window.open('http://www.baidu.com', '_parent');" />
		<input type="button" value="开启百度(顶级窗口)" onclick="window.open('http://www.baidu.com', '_top');" />
		
		<input type="button" value="打开新窗口"  onclick="window.open('02.html')"/>
	</body>
</html>
```

```html
<!--02.html-->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<input type="button" value="关闭当前窗口" onclick="window.close();" />
	</body>
</html>
```

##### 2.3.2 弹出消息框和确认框

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>弹出消息框和确认框</title>
	</head>
	<body>
		<script type="text/javascript">
			function del(){
				/*
				var ok = window.confirm("亲，确认删除数据吗？");
				if(ok){
					alert("delete data ....");
				}
				*/
			    if(window.confirm("亲，确认删除数据吗？")){
			    	alert("delete data ....");
			    }
			}
		</script>
		<input type="button" value="弹出消息框" onclick="window.alert('消息框!')" />
		<!--删除操作的时候都要提前先得到用户的确认。-->
		<input type="button" value="弹出确认框(删除)" onclick="del();" />
	</body>
</html>
```

##### 2.3.3 当前窗口设置为顶级窗口

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>当前窗口设置为顶级窗口</title>
		<!--窗体-->
		<!-- 
		<frameset cols="30%,*">
			<frame src="http://www.baidu.com" />
			<frame src="005-child-window.html" />
		</frameset> 
		-->
	</head>
	<body>
		<!--在当前窗口中隐藏的内部窗体。-->
		<!-- <iframe src="http://www.baidu.com"></iframe> -->
		<iframe src="05.html"></iframe>
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>05.html</title>
	</head>
	<body>
		child window.
		<script type="text/javascript">
			window.onload = function(){
				var btn = document.getElementById("btn");
				btn.onclick = function(){
					if(window.top != window.self){
						//window.top = window.self;
						window.top.location = window.self.location;
					}
				}
			}
		</script>
		<input type="button" value="将当前窗口设置为顶级窗口" id="btn" />
	</body>
</html>
```

##### 2.3.4 历史记录-history

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<a href="07.html">07.html页面</a>
		<input type="button" value="前进" onclick="window.history.go(1)"/> 
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>07.html</title>
	</head>
	<body>
		07 page!
		<input type="button" value="后退" onclick="window.history.back()" />
		<input type="button" value="后退" onclick="window.history.go(-1)" />
	</body>
</html>
```

##### 2.3.5 设置浏览器地址栏上的URL

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>设置浏览器地址栏上的URL</title>
	</head>
	<body>
		<script type="text/javascript">
			function goBaidu(){
				//var locationObj = window.location;
				//locationObj.href = "http://www.baidu.com";
				
				// window.location.href = "http://www.jd.com";
				// window.location = "http://www.126.com";
				
				//document.location.href = "http://www.sina.com.cn";
				document.location = "http://www.tmall.com";
			}
		</script>
		
		<input type="button" value="天猫" onclick="goBaidu();"/>
		<input type="button" value="baidu" onclick="window.open('http://www.baidu.com');" />
	</body>
</html>

<!--
总结，有哪些方法可以通过浏览器往服务器发请求？
1、表单form的提交。
2、超链接。<a href="http://localhost:8080/oa/save?username=zhangsan&password=123">用户只能点击这个超链接</a>
3、document.location
4、window.location
5、window.open("url")
6、直接在浏览器地址栏上输入URL，然后回车。（这个也可以手动输入，提交数据也可以成为动态的。）
		
以上所有的请求方式均可以携带数据给服务器，只有通过表单提交的数据才是动态的。
-->
```

#### 2.4 JSON

##### 2.4.1 JSON概述

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JSON</title>
	</head>
	<body>
		<script type="text/javascript">
			/*
				1、什么是JSON，有什么用？
					JavaScript Object Notation（JavaScript对象标记），简称JSON。(数据交换格式)
					JSON主要的作用是：一种标准的数据交换格式。（目前非常流行，90%以上的系统，系统A与系统B交换数据的话，都是采用JSON。）
				2、JSON是一种标准的轻量级的数据交换格式。特点是：
					体积小，易解析。
				3、在实际的开发中有两种数据交换格式，使用最多，其一是JSON，另一个是XML。
					XML体积较大，解析麻烦，但是有其优点是：语法严谨。（通常银行相关的系统之间进行数据交换的话会使用XML。）
				4、JSON的语法格式：
					var jsonObj = {
						"属性名" : 属性值,
						"属性名" : 属性值,
						"属性名" : 属性值,
						"属性名" : 属性值,
						....
					};
				5、XML的语法格式：
				<?xml version="1.0" encoding="GBK"?>
				<!--
					HTML和XML有一个父亲：SGML（标准通用的标记语言。）
					HTML主要做页面展示：所以语法松散。很随意。
					XML主要做数据存储和数据描述的：所以语法相当严格。
				-->
				<students>
					<student sno="110">
						<sname>张三</sname>
						<sex>男</sex>
					</student>
					<student sno="120">
						<sname>李四</sname>
						<sex>男</sex>
					</student>
					<student sno="130">
						<sname>王五</sname>
						<sex>男</sex>
					</student>
				</students>
			*/
		   // 创建JSON对象(JSON也可以称为无类型对象。轻量级，轻巧。体积小。易解析。)
		    var studentObj = {
				"sno" : "110",
				"sname" : "张三",
				"sex" : "男"
			};
			// 访问JSON对象的属性
			alert(studentObj.sno + "," + studentObj.sname + "," + studentObj.sex);
			
			// 之前没有使用JSON的时候,定义类,创建对象,访问对象的属性.
			Student = function(sno,sname,sex){
				this.sno = sno;
				this.sname = sname;
				this.sex = sex;
			}
			var stu = new Student("111","李四","男");
			alert(stu.sno + "," + stu.sname + "," + stu.sex);
			
			// JSON数组
			var students = [
				{"sno":"110","sname":"张三","sex":"男"},
				{"sno":"120","sname":"李四","sex":"男"},
				{"sno":"130","sname":"王五","sex":"男"}
			];
			// 遍历
			for(var i = 0; i < students.length; i++){
				var stuObj = students[i];
				alert(stuObj.sno + "," + stuObj.sname + "," + stuObj.sex);
			}
		</script>
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>复杂一些的JSON对象</title>
	</head>
	<body>
		<script type="text/javascript">
			var user = {
				"usercode" : 110,
				"username" : "张三",
				"sex" : true,
				"address" : {
					"city" : "北京",
					"street" : "大兴区",
					"zipcode" : "12212121",
				},
				"aihao" : ["movie","comic","tv"]
			};
			// 访问人名以及居住的城市
			alert(user.username + "居住在" + user.address.city);
			
			//请自行设计JSON格式的数据，这个JSON格式的数据可以描述整个班级中每一个学生的信息，以及总人数信息。
		    var jsonData = {
				"total": 3,
				"students": [
					{"name":"zhangsan","birth":"1980-10-20"},
					{"name":"lisi","birth":"1981-10-20"},
					{"name":"wangwu","birth":"1982-10-20"}
				]
			};
		</script>
	</body>
</html>
```

##### 2.4.2 eval函数

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>eval函数</title>
	</head>
	<body>
		<!--
			JSON是一种行业内的数据交换格式标准。
			JSON在JS中以JS对象的形式存在。
		-->
		<script type="text/javascript">
			/*
			eval函数的作用是：
				将字符串当做一段JS代码解释并执行。
			*/
		   window.eval("var i = 100;");//相当于i = 100;
		   alert("i = " + i); // i = 100
		  
		   // java连接数据库,查询数据之后,将数据在java程序中拼接成JSON格式的“字符串”,将json格式的字符串响应到浏览器
		   // 也就是说java响应到浏览器上的仅仅是一个"JSON格式的字符串",还不是一个json对象.
		   // 可以使用eval函数,将json格式的字符串转换成json对象.
		   var fromJava = "{\"name\":\"zhangsan\",\"password\":\"123\"}"; //这是java程序给发过来的json格式的"字符串"
		   // 将以上的json格式的字符串转换成json对象
		   window.eval("var jsonObj = " + fromJava);
		   // 访问json对象
		   alert(jsonObj.name + "," + jsonObj.password); // 在前端取数据.
		   
		   /*
			面试题：
				在JS当中：[]和{}有什么区别？
					[] 是数组。
					{} 是JSON。
				
				java中的数组：int[] arr = {1,2,3,4,5};
				JS中的数组：var arr = [1,2,3,4,5];
				JSON：var jsonObj = {"email" : "zhangsan@123.com","age":25};
		   */
		  
		   var json = {
			   "username" : "zhangsan"
		   };
		   // JS中访问json对象的属性
		   alert(json.username);
		   // JS中访问json对象的属性
		   alert(json["username"]);
		</script>
	</body>
</html>
```

##### 2.4.3 设置table的tbody

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>设置table的tbody</title>
	</head>
	<body>
		<script type="text/javascript">
			// 有这些json数据
			var data = {
				"total" : 4,
				"emps" : [
					{"empno":7369,"ename":"SMITH","sal":800.0},
					{"empno":7361,"ename":"SMITH2","sal":1800.0},
					{"empno":7360,"ename":"SMITH3","sal":2800.0},
					{"empno":7362,"ename":"SMITH4","sal":3800.0}
				]
			};
			
			// 希望把数据展示到table当中.
			window.onload = function(){
				var displayBtnElt = document.getElementById("displayBtn");
				displayBtnElt.onclick = function(){
					var emps = data.emps;
					var html = "";
					for(var i = 0; i < emps.length; i++){
						var emp = emps[i];
						html += "<tr>";
						html += "<td>"+emp.empno+"</td>";
						html += "<td>"+emp.ename+"</td>";
						html += "<td>"+emp.sal+"</td>";
						html += "</tr>";
					}
					document.getElementById("emptbody").innerHTML = html;
					document.getElementById("count").innerHTML = data.total;
				}
			}
		</script>
		
		<input type="button" value="显示员工信息列表" id="displayBtn" />
		<h2>员工信息列表</h2>
		<hr>
		<table border="1px" width="50%">
			<tr>
				<th>员工编号</th>
				<th>员工名字</th>
				<th>员工薪资</th>
			</tr>
			<tbody id="emptbody">
				<!--
				<tr>
					<td>7369</td>
					<td>SMITH</td>
					<td>800</td>
				</tr>
				<tr>
					<td>7369</td>
					<td>SMITH</td>
					<td>800</td>
				</tr>
				<tr>
					<td>7369</td>
					<td>SMITH</td>
					<td>800</td>
				</tr>
				-->
			</tbody>
		</table>
		总共<span id="count">0</span>条数
	</body>
</html>
```
# Http网络协议

### 一、网络协议包

```
1.在网络中传递信息都是以【二进制】形式存在的。
2.接收方【浏览器/服务器】在接收信息后，要做第一件事
  就是将【二进制数据】进行编译【文字，图片，视频，命令】
3.传递信息数据量往往比较巨大，导致接收方很难在一组连续
  二进制得到对应数据
  比如 浏览器发送一个请求： http://192.168.100.2:8080/index.html
	  这个请求信息以二进制形式发送 01010101010110101010101101010
	  Http服务器很难从二进制数据得到相关信息
 4.网络协议包一组有规律二进制数据，在这组数据存在了固定空间
	每一个空间专门存放特定信息，这样接收方在接收网络协议包之后
	就可以到固定空间得到对应信息，网络协议包出现极大降低了
	接收方对接收二进制数据编译难度

	【0000（ip地址）0000（端口号）0000（资源文件名）0000】
```

### 二、常见网络协议

- **FTP**网络协议包
- **HTTP**网络协议包

### 三、HTTP网络协议包

- 在基于B/S结构下互联网通信信过程中，所有在网络中传递信息都是保存在Http网络协议包
- 分类：
  1. Http 请求协议包
  2. Http 响应协议包

### 四、Http请求协议包与Http响应协议包

- **Http请求协议包**：在浏览器准备发送请求时，负责创建一个Http请求协议包，浏览器将请求信息以二进制形式保存在Http请求协议包各个空间，由浏览器负责将Http请求协议包推送到指定服务端计算机
- **Http响应协议包**：Http服务器在定位到被访问的资源文件之后。负责创建一个Http响应协议包，Http服务器将定位文件内容或则文件命令以二进制形式写入到Http响应协议包各个空间，由Http服务器负责将Http响应协议包推送回发起请求的浏览器上。

### 五、Http请求协议包内部空间

1. 按照自上而下划分，分为四个空间

2. 空间划分：

   - 请求行：[

     ​		url: 请求地址（http://localhost:8080/index.html）

     ​		method:请求方式（POST/GET）

   ​	   ]

   - 请求头：[

     ​		请求参数信息【GET】

     ]

   - 空白行：[

     ​		没有任何内容，起到隔离作用

     ]

   - 请求体：[

     ​		请求参数信息【POST】

     ]

### 六、Http响应协议包内部结构

1. 按照自上而下划分，分为四个空间

2. 空间划分：

   - 状态行：[

     ​		HTTP状态码

     ]

   - 响应头：[

     ​		content-type: 指定浏览器采用对应编译器对响应体二进制数据进行解析

     ]

   - 空白行：[

     ​		没有任何内容，起到隔离作用

     ]

   - 响应体：[

     ​		可能被访问静态资源文件内容

     ​		可能被访问的静态资源文件命令

     ​		可能被访问的动态资源文件运行结果
     ​	    ***都是以二进制形式***

     ]

![image-20211211205920149](02Http网络协议与Tomcat.assets\image-20211211205920149.png)

### 七、Http服务器分类

- JBOSS 服务器
- Glassfish 服务器
- Jetty 服务器
- Weblogic 服务器
- Websphere 服务器
- Tomcat 服务器

# Tomcat服务器

### 一、Tomcat服务器安装与配置

Tomcat服务器安装只需去官网下载安装，下载完成后直接解压即可

配置：

​	变量名:JAVA_HOME    变量值:X:\newJava\JDK8

​	变量值:CATALINA_HOME    变量值:X:\newTool\Tomcat\apache-tomcat-9.0.56	

配置：

​		JAVA_HOME:

![image-20211211212357300](02Http网络协议与Tomcat.assets\image-20211211212357300.png)

![image-20211211212404222](02Http网络协议与Tomcat.assets\image-20211211212404222.png)

### 二、Tomcat启动关闭

1. 启动关闭命令存放位置：Tomcat 安装位置/bin

2. 启动命令：startup.bat

3. 关闭命令：shutdown.bat

   ![image-20211211212831439](02Http网络协议与Tomcat.assets\image-20211211212831439.png)

4. 进入cmd，进入对应的文件路径目录下，输入startup启动Tomcat服务器，输入shutdown关闭Tomcat服务器

### 三、模拟第一次互联网通信

1. 在Tomcat 安装地址/webapps 文件夹下创建一个网站【myWeb】

   ![image-20211212161015729](02Http网络协议与Tomcat.assets\image-20211212161015729.png)

2. 将一个静态资源文件添加到网站【beaytify.jpg】  

   ![image-20211212161344049](02Http网络协议与Tomcat.assets\image-20211212161344049.png)

3. 启动Tomcat，必须先shutdown关闭Tomcat，然后startup启动Tomcat  

   ![image-20211212162238912](02Http网络协议与Tomcat.assets\image-20211212162238912.png)

   ![image-20211212162246421](02Http网络协议与Tomcat.assets\image-20211212162246421.png)

4. 启动浏览器【谷歌】，命令浏览器向 Tomcat 索要 beaytify.jpg

   URL格式：网络协议包://服务端计算机IP地址:Http服务端端口号/网络名/资源文件名称

   http://localhost:8080/myWeb/beaytify.jpg

   ![image-20211212162553403](02Http网络协议与Tomcat.assets\image-20211212162553403.png)

![image-20211212162559103](02Http网络协议与Tomcat.assets\image-20211212162559103.png)

![image-20211212162602416](02Http网络协议与Tomcat.assets\image-20211212162602416.png)

### 四、IDEA配置管理Tomcat

1. 通知IDEA管理的Tomcat位置

   a. File -> Setting

   ![image-20211212163431292](02Http网络协议与Tomcat.assets\image-20211212163431292.png)

   b. Build,Execution,Deployment -> Application Servers 

   ![image-20211212163441403](02Http网络协议与Tomcat.assets\image-20211212163441403.png)

   c. OK

   ![image-20211212163453605](02Http网络协议与Tomcat.assets\image-20211212163453605.png)

   d. OK

   ![image-20211212163644322](02Http网络协议与Tomcat.assets\image-20211212163644322.png)

2.  设置 Tomcat 启动与关闭按钮

   a. Run -> Edit Configurations

   ![image-20211212163854268](02Http网络协议与Tomcat.assets\image-20211212163854268.png)

   b.  + -> Tomcat Server -> Local

   ![image-20211212164108505](02Http网络协议与Tomcat.assets\image-20211212164108505.png)

   c. OK

   ![image-20211212164329794](02Http网络协议与Tomcat.assets\image-20211212164329794.png)

   d. 

   ![image-20211212164710194](02Http网络协议与Tomcat.assets\image-20211212164710194.png)

   e.  点击启动或调试按钮后

   ![image-20211212165340420](02Http网络协议与Tomcat.assets\image-20211212165340420.png)

# Servlet规范

### 一、Servlet规范介绍

1. Servlet规范来自JAVAEE规范中的一种
2. 作用：
   - 在Servlet规范中，指定【动态资源文件】开发步骤
   - 在Servlet规范中，指定Http服务器调用动态资源文件规则
   - 在Servlet规范中，指定Http服务器管理动态资源文件实例对象规则

### 二、Servlet接口实现类

1. Servlet接口来自于Servlet规范下一个接口，这个接口存在Http服务器所提供的jar包

2. Tomcat服务器下lib文件有一个servlet-api.jar存放Servlet接口（javax.servlet.Servlet接口）

3. Servlet规范中任务，Http服务器能调用的【动态资源文件】必须是一个Servlet接口实现类

   ```java
   例子：
   class Student{
   	//不是动态资源文件，Tomcat无权调用
   }
   
   class Teacher implements Servlet{
   	//合法动态资源文件，Tomcat有权利调用
   	Servlet obj = new Teacher();
   	obj.doGet()
   }
   ```

### 三、Servlet接口实现类开发步骤

- 第一步：创建一个Java类继承于HttpServlet，使之成为一个Servlet接口实现类

- 第二部：重写HttpServlet父类两个方法。doGet() 或者 doPost()

  ​			浏览器 ---> get请求方式  --->  oneServlet.doGet();

  ​			浏览器 ---> post请求方式  --->  oneServlet.doPost();

- 第三部：将Servlet接口实现类相关信息【注册】到Tomcat服务器

  ​			【网站】--->【src.main.webapp】--->【Web-INF】---> web.xml

  ```xml
  <!--将Servlet接口实现类类路径地址交给Tomcat-->
  <servlet>
  	<servlet-name>mm</servlet-name> <!--声明一个变量存储servlet接口实现类类路径-->
  	<servlet-class>com.xukang.controller.OneServlet</servlet-class><!--声明servlet接口实现类类路径-->
  </servlet>
  
  Tomcat String mm = "com.xukang.controller.OneServlet"
  <!--为了降低用户访问Servlet接口实现类难度，需要设置简短请求别名-->
  <servlet-mapping> 
  	<servlet-name>mm</servlet-name>
  	<url-pattern>/one</url-pattern> <!--设置简短请求别名,别名在书写时必须以"/"为开头-->
  </servlet-mapping>
  
  <!--
  如果现在浏览器向Tomcat索要OneServlet时地址
  http://localhost:8080/myWeb/one
  -->
  ```

```java
package com.xukang.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
/**
 * 子类继承于父类，父类实现于A接口
 * 此时，子类也是A接口实现类
 *
 * 抽象类作用： 降低接口实现类对接口实现过程的难度
 *           将接口中不需要使用抽象方法就给抽象类进行完成
 *           这样接口实现类只需要对接口需要实现的方法进行重写
 *
 *  Servlet接口
 *          init()
 *          getServletConfig()
 *          getServletInfo()
 *          destory() ------ 四个方法对 Servlet接口实现类没用
 *          service() ---- 有用
 *
 *   Tomcat根据Servlet规范调用Servlet接口实现类规则：
 *          1. Tomcat有权创建Servlet接口实现类实例对象
 *              Servlet oneServlet = new OneServlet();
 *          2. Tomcat根据实例对象调用service()方法处理当前请求
 *              oneServlet.service();//此时service()方法中this====oneServlet
 *
 *   OneServlet --> 根据父类实现的service()方法，去重写doGet(),doPost()等方法
 *   继承于
 *   (abstract)HttpServlet ---> 实现了service()方法
 *   继承于
 *   (abstract)GenericServlet --> 实现了 init(),destory(),getServletInfo(),getServletConfig() 方法
 *   实现于
 *   Servlet接口
 *
 *   通过父类决定在何种情况下调用子类中方法，设计模式中称为模板设计模式
 *
 *   HttpServlet:
 *      service(){
 *          if(请求方式 == Get){
 *              this.doGet();
 *          }else if(请求方式 == POST){
 *              this.doPost();
 *          }...
 *      }
 *   OneServlet: doGet()   doPost()
 *
 *   Servlet oneServlet = new OneServlet();
 *   oneServlet.service();
 */
public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("OneServlet类针对浏览器发送GET请求方式处理");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("OneServlet类针对浏览器发送POST请求方式处理");
    }
}
```

```xml
<!--web.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--servlet接口实现类类路径地址交给Tomcat-->
    <servlet>
        <servlet-name>oneServlet</servlet-name>
        <servlet-class>com.xukang.controller.OneServlet</servlet-class>
    </servlet>

    <!--为servlet接口实现类提供简短别名-->
    <servlet-mapping>
        <servlet-name>oneServlet</servlet-name>
        <url-pattern>/one</url-pattern>
    </servlet-mapping>
</web-app>
```

```java
// this 指向问题
public class Father {
    public void service(String method){
        if("get".equals(method)){
            /*
            子类如果重写了doGet()方法，那么this====子类实例对象
            如果没有重写doGet()方法，那么this====Father
            */
            this.doGet();
        }else if("post".equals(method)){
            this.doPost();
        }
    }
    protected void doPost() {
        System.out.println("Father doPost is running ...");
    }
    protected void doGet() {
        System.out.println("Father doGet is running ...");
    }
}

public class Son extends Father{
    @Override
    protected void doPost() {
        System.out.println("Son doPost is running ...");
    }
    @Override
    protected void doGet() {
        System.out.println("Son doGet is running ...");
    }
}

public class TestMain {
    public static void main(String[] args) {
        Father son = new Son();
        son.service("get");
        son.service("post");
    }
}

/*
输出：
    Son doGet is running ...
    Son doPost is running ...
*/
```

### 四、Servlet对象生命周期

1. 网站中所有的Servlet接口实现类的实例对象，只能由Http服务器负责创建。开发人员不能手动创建Servlet接口实现类的实例对象。

2. 在默认的情况下，Http服务器接收到对于当前Servlet接口实现类第一次请求时，自动创建这个Servlet接口实现类的实例对象。

   在手动配置情况下，要求Http服务器在启动时自动创建某个Servlet接口实现类的实例对象

   ```xml
   <servlet>
   	<servlet-name>mm</servlet-name> <!--声明一个变量存储servlet接口实现类类路径-->
   	<servlet-class>com.bjpowernode.controller.OneServlet</servlet-class>
   	<load-on-startup>30<load-on-startup><!--填写一个大于0的整数即可，Tomcat启动时对应的实现类的实例对象被创建出来-->
   </servlet>
   ```

3. 在Http服务器运行期间，一个Servlet接口实现类只能被创建出一个实例对象。

4. 在Http服务器关闭时刻，自动将网站中所有的Servlet对象进行销毁。

```java
package com.example.controller;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;

public class OneServlet extends HttpServlet {
    public OneServlet(){
        System.out.println("OneServlet类被创建实现类");
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("OneServlet doGet is run...");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

```java
package com.example.controller;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class TwoServlet extends HttpServlet {
    public TwoServlet(){
        System.out.println("TwoServlet类被创建实现类");
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("TwoServlet doGet is run...");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>OneServlet</servlet-name>
        <servlet-class>com.example.controller.OneServlet</servlet-class>
    </servlet>

    <servlet>
        <servlet-name>TwoServlet</servlet-name>
        <servlet-class>com.example.controller.TwoServlet</servlet-class>
        <!--通知Tomcat在启动时负责创建TwoServlet实例对象-->
        <load-on-startup>9</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>OneServlet</servlet-name>
        <url-pattern>/one</url-pattern>
    </servlet-mapping>
    
    <servlet-mapping>
        <servlet-name>TwoServlet</servlet-name>
        <url-pattern>/two</url-pattern>
    </servlet-mapping>
</web-app>
```

启动Tomcat后

![image-20211212192519808](03Servlet规范.assets\image-20211212192519808.png)

### 五、HttpServletResponse接口

1. 介绍：
   - HttpServletResponse接口来自于Servlet规范中，在Tomcat中存在于servlet-api.jar
   - HttpServletResponse接口实现类由Http服务器负责提供
   - HttpServletResponse接口负责将 doGet()/doPost() 方法执行结果写入到【响应体】交给浏览器
   - 开发人员习惯于将HttpServletResponse接口修饰的对象称为【响应对象】
2. 主要功能
   - 将执行结果以二进制形式写入到【响应体】
   - 设置响应头中[content-type]属性值，从而控制浏览器使用对应编译器将响应体的 二进制数据 编译为【文字，图片，视频，命令】
   - 设置响应头中【location】属性，将一个请求地址赋值给location，从而控制浏览器向指定服务器发送请求

```java
package com.example.controller;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String result = "Hello World";//执行结果
        //=====响应对象将结果写入到响应体===== start
        //1.通过响应对象，向Tomcat索要输出流
        PrintWriter out = response.getWriter();
        //2.通过输出流，将执行结果以二进制形式写入到响应体
        out.write(result);
    }
    //doGet()执行完毕
    //Tomcat将响应包推送给浏览器
}
```

```java
package com.example.controller;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class TwoServlet extends HttpServlet {
    /*
    * 问题描述：out.write(50); 浏览器收到数据是2，不是50
    *
    * 问题原因：
    *       out.write()方法可以将【字符】,【字符串】,【ASCII码】写入到响应体
    *       【ASCII码】
    *               a ------- 97
    *               2 ------- 50
    *       out.write()收到数字的时候，会把数字当做ASCII码去执行代码
    *
    * 问题解决方案：
    *       在实际的开发过程中，都是通过out.print()将真实数据写入到响应体.
    * */
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        int money = 50;//执行结果

        PrintWriter out = response.getWriter();
        out.write(money);//2
        out.println();
        out.print(money);//50
    }
    //doGet()执行完毕
    //Tomcat将响应包推送给浏览器
}
```

```java
package com.example.controller;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class ThreeServlet extends HttpServlet {
    /*
    * 问题描述：Java<br/>MySql<br/>HTML<br/>
    *         浏览器在接收到响应结果时，将<br/>作为文字内容在
    *         窗口中显示出来，没有将<br/>当做HTMl标签命令来执行
    *
    * 问题原因：
    *       浏览器在接收响应包之后，根据【响应头中的 content-type】属性的
    *       值，来采用对应【编译器】对【响应体中二进制内容】进行编译处理
    *
    *       在默认情况下，content-type属性值为"text" content-type = "text";
    *       此时浏览器将会采用【文本编译器】对响应体二进制数据进行解析
    *
    * 解决方案：
    *       一定要在得到输出流之前，通过响应对象对响应头中content-type属性进行一次
    *       重新赋值用于指定浏览器采用正确编译器
    *
    * */
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //<br/>为Html中换行命令
        //既有文字信息，又有HTML标签命令
        String result = "Java<br/>MySql<br/>HTML<br/>";
        String resule2 = "动漫<br/>电影<br/>TV<br/>";
        //PrintWriter out = response.getWriter();
        //out.print(result);//Java<br/>MySql<br/>HTML<br/>

        //设置响应头content-type
        response.setContentType("text/html;charset=utf-8");
        //向Tomcat索要输出流
        PrintWriter out2 = response.getWriter();
        //通过输出流将结果写入到响应体
        out2.print(result);
        out2.print(resule2);
    }
}
```

```java
package com.example.controller;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class FourServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String result = "https://www.baidu.com";

        //通过响应对象，将地址赋值给响应头中location属性
        response.sendRedirect(result);//[响应头 location="https://www.baidu.com"]
    }
    /*
    *   浏览器在接收到响应包之后，如果发现响应头中存在location属性
    *   自动通过地址栏向location指定网站发送请求
    *
    *   sendRedirect方法远程控制浏览器请求行为【请求地址，请求方式，请求参数】
    * */
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>OneServlet</servlet-name>
        <servlet-class>com.example.controller.OneServlet</servlet-class>
    </servlet>
    <servlet>
        <servlet-name>TwoServlet</servlet-name>
        <servlet-class>com.example.controller.TwoServlet</servlet-class>
    </servlet>
    <servlet>
        <servlet-name>ThreeServlet</servlet-name>
        <servlet-class>com.example.controller.ThreeServlet</servlet-class>
    </servlet>
    <servlet>
        <servlet-name>FourServlet</servlet-name>
        <servlet-class>com.example.controller.FourServlet</servlet-class>
    </servlet>
    <!--起别名-->
    <servlet-mapping>
        <servlet-name>OneServlet</servlet-name>
        <url-pattern>/one</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>TwoServlet</servlet-name>
        <url-pattern>/two</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>ThreeServlet</servlet-name>
        <url-pattern>/three</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>FourServlet</servlet-name>
        <url-pattern>/four</url-pattern>
    </servlet-mapping>
</web-app>
```

### 六、HttpServletRequest接口

1. 介绍
   - HttpServletRequest接口来自于Servlet规范中，在Tomcat中存在servlet-api.jar
   - HttpServletRequest接口实现类由Http服务器负责提供
   - HttpServletRequest接口负责在doGet()/doPost()方法运行时读取Http请求协议包中信息
   - 开发人员习惯于将HttpServletRequest接口修饰的对象称为【请求对象】
2. 作用
   - 可以读取Http请求协议包中【请求行】信息
   - 可以读取保存在Http请求协议包中【请求头】或则【请求体】中请求参数信息
   - 可以代替浏览器向Http服务器申请资源文件调用

```java
/***
 * 读取请求行的信息
 */
public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.通过请求对象，读取【请求行】中【url】信息
        String url = request.getRequestURL().toString();
        //2.通过请求对象，读取【请求行】中【method】信息
        String method = request.getMethod();
        //3.通过请求对象，读取【请求行】中uri信息
        /*
         * URI：资源文件精准定位地址，在请求行并没有URI这个属性。
         *      实际上URL中截取一个字符串，这个字符串格式"/网站名/资源文件名"
         *      URI用于让Http服务器对被访问的资源文件进行定位
         */
        String uri = request.getRequestURI();

        System.out.println("URL "+url);//URL http://localhost:8080/myWeb/one
        System.out.println("method "+method);//method GET
        System.out.println("URI "+uri);//URI /myWeb/one
    }
}
```



```java
public class TwoServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.通过请求对象获得【请求头】中【所有请求参数名】
        //将所有请求参数名称保存到一个枚举对象进行返回
        Enumeration<String> parameterNames = request.getParameterNames();
        while(parameterNames.hasMoreElements()){
            String paramName = (String) parameterNames.nextElement();
            //2.通过请求对象读取指定的请求参数的值
            String value = request.getParameter(paramName);
            System.out.println("请求参数名 = " + paramName + ",请求参数值 = " + value);
            //请求参数名 = userName,请求参数值 = jack
            //请求参数名 = password,请求参数值 = 123
        }
    }
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <center>
        <a href="/myWeb/two?userName=jack&password=123">通过超链接访问TwoServlet别携带请求参数</a>
    </center>
</body>
</html>
```



```java
/*
 *  问题：
 *      以GET方式发送中文参数内容“老杨是正经人”时，得到正常结果
 *      以POST方式发送中文参数内容"老崔是个男人"，得到【乱码】“è?????????????·???”
 *
 *  原因:
 *      浏览器以GET方式发送请求,请求参数保存在【请求头】,在Http请求协议包到达Http服务器之后，第一件事就是进行解码
 *      请求头二进制内容由Tomcat负责解码，Tomcat9.0默认使用【utf-8】字符集，可以解释一切国家文字
 *
 *      浏览器以POST方式发送请求，请求参数保存在【请求体】,在Http请求协议包到达Http服务器之后，第一件事就是进行解码
 *      请求体二进制内容由当前请求对象（request）负责解码。request默认使用[ISO-8859-1]字符集，一个东欧语系字符集
 *      此时如果请求体参数内容是中文，将无法解码只能得到乱码
 *
 *  解决方案:
 *      在Post请求方式下，在读取请求体内容之前，应该通知请求对象使用utf-8字符集对请求体内容进行一次重新解码
 */
public class ThreeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //通过请求对象，读取【请求头】参数信息
        String userName = request.getParameter("userName");
        System.out.println("从请求头得到参数值 " + userName);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //通知请求对象，使用utf-8字符集对请求体二进制内容进行一次重写解码
        request.setCharacterEncoding("utf-8");
        //通过请求对象，读取【请求体】参数信息
        String value = request.getParameter("userName");
        System.out.println("从请求体得到参数值 "+value);
    }
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <center>
        <form action="/myWeb/three" method="get">
            请求参数:<input type="text" name="userName" /><br/>
            <input type="submit" value="get方式访问ThreeServlet">
        </form>

        <form action="/myWeb/three" method="post">
          请求参数:<input type="text" name="userName" /><br/>
          <input type="submit" value="post方式访问ThreeServlet">
        </form>
    </center>
</body>
</html>
```

### 七、请求对象与响应对象生命周期

1. 在Http服务器接收到浏览器发送的【Http请求协议包】之后，自动为当前的【Http请求协议包】生成一个【请求对象】和一个【响应对象】

2.  在Http服务器调用doGet/doPost方法时，负责将【请求对象】和【响应对象】作为实参传递到方法，确保doGet/doPost正确执行

3. 在Http服务器准备推送Http响应协议包之前，负责将本次请求关联的【请求对象】和【响应对象】销毁

   ​	 ***【请求对象】和【响应对象】生命周期贯穿一次请求的处理过程中

   ​	 ***【请求对象】和【响应对象】相当于用户在服务端的代言人

### 八、在线考试管理系统

##### 1、准备工作

```
任务：在线考试管理系统----用户信息管理模块
子任务:
      用户信息注册
      用户信息查询
      用户信息删除
      用户信息更新
准备工作:
      1.创建用户信息表 Users.frm
      CREATE TABLE Users(
        userId int  primary key auto_increment, #用户编号
        userName varchar(50),    #用户名称
        password varchar(50),    #用户密码
        sex      char(1),        #用户性别 '男' 或者 '女'
        email    varchar(50)     #用户邮箱
      )
      auto_increment,自增序列    i++
      在插入时，如果不给定具体用户编号，此时根据auto_increment的值递增添加
      2.在src下 com.example.entity.Users 实体类
      3.在src下 com.example.util.JdbcUtil 工具类【复用】
      4.在web下WEB-INF下创建lib文件夹  存放mysql提供JDBC实现jar包
```

```java
//Users类 
package com.example.entity;
public class Users {
    private Integer userId;
    private String userName;
    private String passWord;
    private String sex;
    private String email;

    public Users() {
    }
    public Users(Integer userId, String userName, String passWord, String sex, String email) {
        this.userId = userId;
        this.userName = userName;
        this.passWord = passWord;
        this.sex = sex;
        this.email = email;
    }

    public Integer getUserId() {
        return userId;
    }
    public void setUserId(Integer userId) {
        this.userId = userId;
    }
    public String getUserName() {
        return userName;
    }
    public void setUserName(String userName) {
        this.userName = userName;
    }
    public String getPassWord() {
        return passWord;
    }
    public void setPassWord(String passWord) {
        this.passWord = passWord;
    }
    public String getSex() {
        return sex;
    }
    public void setSex(String sex) {
        this.sex = sex;
    }
    public String getEmail() {
        return email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
}
```

```java
package com.example.util;
import java.sql.*;
/**
 * JDBC工具类，简化JDBC编程
 */
public class JdbcUtil {
    /**
     * 工具类的构造方法都是私有的
     * 因为工具类当中的方法都是静态的，不需要new对象，直接采用类名调用
     */
    private JdbcUtil(){}

    // 静态代码块在类加载时执行，并且只执行一次
    // 注册驱动
    static {
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接对象
     * @return 连接对象
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/bjpowernode","root","111");
        return conn;
    }

    /**
     * 关闭资源
     * @param conn 连接对象
     * @param ps   数据库操作对象
     * @param rs   结果集
     */
    public static void close(Connection conn, Statement ps, ResultSet rs){
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(ps != null){
            try {
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

##### 2、用户信息注册流程图

![image-20211218201009978](03Servlet规范.assets\image-20211218201009978.png)

##### 3、User_Add开发

```html
<!--user_Add.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <center>
        <form action="/myWeb/user/add" method="get">
            <table border="2">
              <tr>
                <td>用户姓名</td>
                <td><input type="text" name="userName"></td>
              </tr>
              <tr>
                <td>用户密码</td>
                <td><input type="password" name="password"></td>
              </tr>
              <tr>
                <td>用户性别</td>
                <td>
                  <input type="radio" name="sex" value="男" />男
                  <input type="radio" name="sex" value="女" />女
                </td>
              </tr>
              <tr>
                <td>用户邮箱</td>
                <td><input type="text" name="email"></td>
              </tr>
              <tr>
                <td><input type="submit" value="用户注册" /></td>
                <td><input type="reset" name="重置" /></td>
              </tr>
            </table>
        </form>
    </center>
</body>
</html>
```

```java
package com.example.controller;

import com.example.dao.UserDao;
import com.example.entity.Users;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class UserAddServlet extends HttpServlet {//别名：/user/add
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.【调用请求对象】读取【请求头】参数信息，得到用户的信息
        String userName, passWord, sex, email;
        userName = request.getParameter("userName");
        passWord = request.getParameter("password");
        sex = request.getParameter("sex");
        email = request.getParameter("email");
        //2.【调用UserDao】将用户信息填充到INSERT命令并借助JDBC规范发送到数据库服务器
        UserDao userDao = new UserDao();
        Users user = new Users(null, userName, passWord, sex, email);
        int result = userDao.add(user);
        //3.【调用响应对象】将【处理结果】以二进制形式写入到响应体
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = null;
        out = response.getWriter();
        if(result == 1){
            out.print("<font style='color:red;font-size:40'>用户注册成功</font>");
        }else {
            out.print("<font style='color:red;font-size:40'>用户注册失败</font>");
        }
    }
    //Tocmat负责销毁【请求对象】和【响应对象】
    //Tomcat负责将Http响应协议包推送到发起请求的浏览器上
    //浏览器根据响应头content-type指定编译器对响应体二进制内容编辑
    //浏览器将编辑后结果在窗口中展示给用户【结束】
}
```

```java
package com.example.dao;

import com.example.entity.Users;
import com.example.util.JdbcUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class UserDao {
    /**
     * 用户注册
     * @return 1---注册成功，0---注册失败
     */
    public int add(Users user){
        Connection connection = null;
        PreparedStatement ps = null;
        int result = 0;
        try {
            connection = JdbcUtil.getConnection();
            String sql = "insert into users(userName,password,sex,email)" +
                    "values(?,?,?,?)";
            ps = connection.prepareStatement(sql);
            ps.setString(1, user.getUserName());
            ps.setString(2, user.getPassWord());
            ps.setString(3, user.getSex());
            ps.setString(4, user.getEmail());
            result = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(connection, ps, null);
        }
        return result;
    }
}
```

![image-20211218210452287](03Servlet规范.assets\image-20211218210452287.png)

![image-20211218210456428](03Servlet规范.assets\image-20211218210456428.png)

![image-20211218210535141](03Servlet规范.assets\image-20211218210535141.png)

##### 4、user_Find开发

![image-20211218212802780](03Servlet规范.assets\image-20211218212802780.png)

```java
package com.example.controller;

import com.example.dao.UserDao;
import com.example.entity.Users;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

public class UserFindServlet extends HttpServlet {//别名：/user/find
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1、[调用UserDao] 将查询命令推送到数据库服务器上，得到所有用户信息【List】
        UserDao userDao = new UserDao();
        List<Users> allUsers = userDao.findAll();
        //2、[调用响应对象] 将用户信息结合<table>标签命令以二进制形式写入到响应体
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("<table border='2' align='center' >");
        out.print("<tr>");
        out.print("<td>用户编号</td>");
        out.print("<td>用户姓名</td>");
        out.print("<td>用户密码</td>");
        out.print("<td>用户性别</td>");
        out.print("<td>用户邮箱</td>");
        out.print("</tr>");
        for(Users user : allUsers){
            out.print("<tr>");
            out.print("<td>" + user.getUserId() +"</td>");
            out.print("<td>" + user.getUserName() +"</td>");
            out.print("<td>******</td>");
            out.print("<td>" + user.getSex() +"</td>");
            out.print("<td>" + user.getEmail() +"</td>");
            out.print("</tr>");
        }
        out.print("</table>");
    }
}
```

```java
//UserDao	
	/**
     * 查询所有用户信息
     * @return list集合存储所有用户信息
     */
    public List<Users> findAll(){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        List<Users> list = new ArrayList<>();
        try {
            conn = JdbcUtil.getConnection();
            String sql = "select * from users";
            ps = conn.prepareStatement(sql);
            rs = ps.executeQuery();
            while(rs.next()){
                Integer userId = rs.getInt("userId");
                String userName = rs.getString("userName");
                String password = rs.getString("password");
                String sex = rs.getString("sex");
                String email = rs.getString("email");
                Users user = new Users(userId,userName,password,sex,email);
                list.add(user);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return list;
    }
```

![image-20211218215352438](03Servlet规范.assets\image-20211218215352438.png)

##### 5、导航页面

```html
<!-- webapp/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<frameset rows="15%,85%">
  <frame name="top" src="top.html">
  <frameset cols="15%,85%">
    <frame name="left" src="left.html">
    <frame name="right">
  </frameset>
</frameset>
</html>
```

```html
<!-- webapp/top.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body style="background-color: green">
  <center>
    <font style="color:red;font-size:40px">在线考试管理系统</font>
  </center>
</body>
</html>
```

```html
<!-- webapp/left.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <ul>
    <li>用户信息管理
      <ol>
        <li><a href="/myWeb/user_Add.html" target="right">用户信息注册</a> </li>
        <li><a href="/myWeb/user/find" target="right">用户信息查询</a></li>
      </ol>
    </li>
    <li>试题信息管理</li>
    <li>考试管理</li>
  </ul>
</body>
</html>
```

![image-20211218222533272](03Servlet规范.assets\image-20211218222533272.png)

![image-20211218222537333](03Servlet规范.assets\image-20211218222537333.png)

##### 6、user_Delete开发

```java
package com.example.controller;

import com.example.dao.UserDao;
import com.example.entity.Users;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;
public class UserFindServlet extends HttpServlet {//别名：user/find
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1、[调用UserDao] 将查询命令推送到数据库服务器上，得到所有用户信息【List】
        UserDao userDao = new UserDao();
        List<Users> allUsers = userDao.findAll();
        //2、[调用响应对象] 将用户信息结合<table>标签命令以二进制形式写入到响应体
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("<table border='2' align='center' >");
        out.print("<tr>");
        out.print("<td>用户编号</td>");
        out.print("<td>用户姓名</td>");
        out.print("<td>用户密码</td>");
        out.print("<td>用户性别</td>");
        out.print("<td>用户邮箱</td>");
        out.print("<td>操作</td>");
        out.print("</tr>");
        for(Users user : allUsers){
            out.print("<tr>");
            out.print("<td>" + user.getUserId() +"</td>");
            out.print("<td>" + user.getUserName() +"</td>");
            out.print("<td>******</td>");
            out.print("<td>" + user.getSex() +"</td>");
            out.print("<td>" + user.getEmail() +"</td>");
            out.print("<td><a href='/myWeb/user/delete?userId=" + user.getUserId() + "' >删除用户</a></td>");
            out.print("</tr>");
        }
        out.print("</table>");
    }
}
```

```java
package com.example.controller;

import com.example.dao.UserDao;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class UserDeleteServlet extends HttpServlet {//别名：/user/delete
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1、【调用请求对象】读取【请求头】参数（用户编号）
        String userId = request.getParameter("userId");
        //2、【调用UserDao】将用户编号填充到delete命令并发送到数据库服务器
        UserDao userDao = new UserDao();
        int result = userDao.deleteUser(userId);
        //3、【调用响应对象】将处理结果以二进制写入到响应体，交给浏览器
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        if(result == 1){
            out.print("<font style='color:red;font-size:40'>用户删除成功</font>");
        } else {
            out.print("<font style='color:red;font-size:40'>用户删除失败</font>");
        }
    }
}
```

```java
//UserDao	
	/**
     * 删除指定用户信息
     * @param userId
     * @return
     */
    public int deleteUser(String userId){
        Connection conn = null;
        PreparedStatement ps = null;
        int result = 0;
        try {
            conn = JdbcUtil.getConnection();
            String sql = "delete from users where userId=?";
            ps = conn.prepareStatement(sql);
            ps.setInt(1, Integer.valueOf(userId));
            result = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtil.close(conn, ps, null);
        }
        return result;
    }
```

![image-20211218225518745](03Servlet规范.assets\image-20211218225518745.png)

![image-20211218225526841](03Servlet规范.assets\image-20211218225526841.png)

##### 7、登录验证

![image-20211219153610753](03Servlet规范.assets\image-20211219153610753.png)

```java
package com.example.controller;

import com.example.dao.UserDao;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class LoginServlet extends HttpServlet {//别名:/myWeb/login
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.调用请求对象对请求体使用utf-8字符集进行重新编辑
        request.setCharacterEncoding("utf-8");
        // 2.调用请求对象读取请求体参数信息
        String userName = request.getParameter("userName");
        String password = request.getParameter("password");
        // 3.调用UserDao将查询验证信息推送到数据库服务器上
        UserDao userDao = new UserDao();
        int result = userDao.ligin(userName, password);
        //4.调用响应对象，根据验证结果将不同资源文件地址写入到响应头，交给浏览器
        if(result == 1){
            response.sendRedirect("/myWeb/index.html");
        } else {
            response.sendRedirect("/myWeb/login_error.html");
        }
    }
}
```

```java
//UserDao.java
	/**
     * 用户登录验证
     * @param userName
     * @param password
     * @return
     */
    public int ligin(String userName, String password){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        int result = 0;
        try {
            conn = JdbcUtil.getConnection();
            String sql = "select count(*) from users where userName=? and password=?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, userName);
            ps.setString(2, password);
            rs = ps.executeQuery();
            while(rs.next()){
                result = rs.getInt("count(*)");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return result;
    }
```

```html
<!--login.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <center>
        <form action="/myWeb/login" method="post">
            <table border="2">
                <tr>
                    <td>登录名</td>
                    <td><input type="text" name="userName" /></td>
                </tr>
                <tr>
                    <td>密码</td>
                    <td><input type="password" name="password" /></td>
                </tr>
                <tr>
                    <td><input type="submit" value="提交" /></td>
                    <td><input type="reset" value="重置" /></td>
                </tr>

            </table>
        </form>
    </center>
</body>
</html>
```

```html
<!--login_error.html-->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<center>
  <font style="color: red;font-size: 30px">登录信息不存在，请重新登录！</font>
  <form action="/myWeb/login" method="post">
    <table border="2">
      <tr>
        <td>登录名</td>
        <td><input type="text" name="userName" /></td>
      </tr>
      <tr>
        <td>密码</td>
        <td><input type="password" name="password" /></td>
      </tr>
      <tr>
        <td><input type="submit" value="提交" /></td>
        <td><input type="reset" value="重置" /></td>
      </tr>
    </table>
  </form>
</center>
</body>
</html>
```

![image-20211219160754847](03Servlet规范.assets\image-20211219160754847.png)

![image-20211219160807770](03Servlet规范.assets\image-20211219160807770.png)

### 九、欢迎资源文件

1. 前提：用户可以记住网站名，但是不会记住网站资源文件名。

2. 默认欢迎资源文件:

   用户发送了一个针对某个网站的【默认请求】时，此时由Http服务器自动从当前网站返回的资源文件

   正常请求： http://localhost:8080/myWeb/index.html

   默认请求： http://localhost:8080/myWeb/

3. Tomcat对于默认欢迎资源文件定位规则

   - 规则位置：Tomcat安装位置/conf/web.xml

   - 规则命令：

     ```xml
     <welcome-file-list>
     	<welcome-file>index.html</welcome-file>
     	<welcome-file>index.htm</welcome-file>
     	<welcome-file>index.jsp</welcome-file>
     </welcome-file-list>
     ```

4. 设置当前网站的默认欢迎资源文件规则

   - 规则位置:  网站/web/WEB-INF/web.xml

   - 规则命令：

     ```xml
     	<!--自定义默认欢迎资源文件-->
         <welcome-file-list>
             <welcome-file>login.html</welcome-file>
             <!--servlet作为默认欢迎文件时，开头斜线必须去掉-->
             <welcome-file>user/find</welcome-file>
         </welcome-file-list>
     ```
   
   - 网站设置自定义默认文件定位规则，此时Tomcat自带定位规则将失效

### 十、Http状态码

1. 介绍：

   - 由三位数字组成的一个符号

   - Http服务器在推送响应包之前，根据本次请求处理情况将Http状态码写入到响应包中【状态行】上

   - 如果Http服务器针对本次请求，返回了对应的资源文件。 通过Http状态码通知浏览器应该如何处理这个结果

     如果Http服务器针对本次请求，无法返回对应的资源文件；通过Http状态码向浏览器解释不能提供服务的原因

2. 分类：

   - 组成	100 --- 599; 分为5个大类

   - 1xx:

     ​	最有特征100：通知浏览器本次返回的资源文件, 并不是一个独立的资源文件，需要浏览器在接收响应包之后，继续向Http服务器所要依赖的其他资源文件

   - 2xx:

     ​	最有特征200：通知浏览器本次返回的资源文件是一个完整独立资源文件，浏览器在接收到之后不需要所要其他关联文件 

   - 3xx:

     ​	最有特征302：通知浏览器本次返回的不是一个资源文件内容，而是一个资源文件地址，需要浏览器根据这个地址自动发起请求来索要这个资源文件

     ​	response.sendRedirect('资源文件地址')写入到响应头中location；而这个行为导致Tomcat将302状态码写入到状态行

     ```java
     public class OneServlet extends HttpServlet {
         protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
               String address = "http://www.baidu.com";
               response.sendRedirect(address); //写入到响应头 location
         }
         //Tomcat在推送响应包之前，看到响应体是空，但是响应头location却存放了一个地址。
         //此时Tomcat将302状态码写入到状态行
         //在浏览器接收到响应包之后，因为302状态码，浏览器不会读取响应体内容，自动根据响应头
         //中location的地址发起第二次请求
     }
     ```

   - 4xx:

     ​	404：通知浏览器，由于在服务端没有定位到被访问的资源文件，因此无法提供帮助

     ​	405：通知浏览器，在服务端已经定位到被访问的资源文件(Servlet)；但是这个Servlet对于浏览器采用的请求方式不能处理

   - 5xx:

     ​	500：通知浏览器，在服务端已经定位到被访问的资源文件(Servlet)；这个Servlet可以接收浏览器采用请求方式，但是Servlet在处理请求期间，由于Java异常导致处理失败

     ```java
     public class OneServlet extends HttpServlet {
         @Override
         protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
             Map map = new HashMap();
             int num = (int) map.get("key1");//抛出空指针异常
             //int a = null; // null值不可能赋值给int
             //Integer b = null; //所有高级引用类型都可以赋值给null
         }
     }
     ```

     ![image-20211219170353080](03Servlet规范.assets\image-20211219170353080.png)

### 十一、多个Servlet之间调用规则

1. 前提条件：

   ​	某些来自于浏览器发送请求，往往需要服务端中多个Servlet协同处理。但是浏览器一次只能访问一个Servlet，导致用户需要手动通过浏览器发起多次请求才能得到服务。
   ​这样增加用户获得服务难度，导致用户放弃访问当前网站

2. 提高用户使用感受规则

   ​	无论本次请求涉及到多少个Servlet,用户只需要【手动】通知浏览器发起一次请求即可

3. 多个Servlet之间调用规则：

   - 重定向解决方案
   - 请求转发解决方案

##### 1、重定向解决方案

1. 工作原理：

   ​	用户第一次通过【手动方式】通知浏览器访问OneServlet；OneServlet工作完毕后，将TwoServlet地址写入到响应头location属性中，导致Tomcat将302状态码写入到状态行。

   ​	在浏览器接收到响应包之后，会读取到302状态。此时浏览器自动根据响应头中location属性地址发起第二次请求，访问TwoServlet去完成请求中剩余任务

   ![image-20211219172245489](03Servlet规范.assets\image-20211219172245489.png)

2. 实现命令：

   ​	response.sendRedirect("请求地址")

   ​	将地址写入到响应包中响应头中location属性

   ```java
   public class OneServlet extends HttpServlet {//别名：/myWeb/one
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           System.out.println("OneServlet 负责 洗韭菜");
           //重定向解决方案:
           response.sendRedirect("/myWeb/two");// [地址格式: /网站名/资源文件名]
       }
   }
   
   public class TwoServlet extends HttpServlet {//别名：/myWeb/two
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           System.out.println("TwoServlet 负责 韭菜炒鸡蛋 ");
       }
   }
   ```

3. 特征

   - 请求地址：

     ​	既可以把当前网站内部的资源文件地址发送给浏览器 （/网站名/资源文件名）
     ​

     ​    也可以把其他网站资源文件地址发送给浏览器 (http://ip地址:端口号/网站名/资源文件名)

   - 请求次数：

     ​	浏览器**至少**发送两次请求，但是只有第一次请求是用户手动发送；后续请求都是浏览器自动发送的。

   - 请求方式：

     ​	重定向解决方案中，通过地址栏通知浏览器发起下一次请求，因此通过重定向解决方案调用的资源文件接收的请求方式一定是【GET】

4. 缺点：

   ​	重定向解决方案需要在浏览器与服务器之间进行多次往返，大量时间消耗在往返次数上，增加用户等待服务时间

##### 2、请求转发解决方案

1. 工作原理：

   ​	用户第一次通过手动方式要求浏览器访问OneServlet；OneServlet工作完毕后，通过当前的请求对象代替浏览器向Tomcat发送请求，申请调用TwoServlet。Tomcat在接收到这个请求之后，自动调用TwoServlet来完成剩余任务。

   ![image-20211219174803969](03Servlet规范.assets\image-20211219174803969.png)

2. 实现命令：请求对象代替浏览器向Tomcat发送请求

   ```java
   //1.通过当前请求对象生成资源文件申请报告对象
   RequestDispatcher report = request.getRequestDispatcher("/资源文件名");//一定要以"/"为开头
   //2.将报告对象发送给Tomcat
   report.forward(当前请求对象，当前响应对象);
   ```

   ```java
   public class OneServlet extends HttpServlet {//别名：/myWeb/one
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           System.out.println("OneServlet 开始工作.....");
           //请求转发方案
           //1.通过当前请求对象生成资源文件申请报告对象
           RequestDispatcher report = request.getRequestDispatcher("/two");
           //2.将报告对象发送给Tomcat
           report.forward(request, response);
       }
   }
   
   public class TwoServlet extends HttpServlet {//别名：/myWeb/two
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           System.out.println("OneServlet 工作完毕后，TwoServlet 开始工作.....");
       }
   }
   ```

3.  优点

   - 无论本次请求涉及到多少个Servlet，用户只需要手动通过浏览器发送一次请求
   - Servlet之间调用发生在服务端计算机上，节省服务端与浏览器之间往返次数增加处理服务速度

4.  特征：

   - 请求次数：在请求转发过程中，浏览器只发送一次请求

   - 请求地址：

     ​	只能向Tomcat服务器申请调用当前网站下资源文件地址
     ​	request.getRequestDispathcer("/资源文件名")    **不要写网站名**

   - 请求方式：

     ​	在请求转发过程中，浏览器只发送一个了个Http请求协议包；参与本次请求的所有Servlet共享同一个请求协议包，因此这些Servlet接收的请求方式与浏览器发送的请求方式保持一致

### 十二、多个Servlet之间数据共享实现方案

- 数据共享：OneServlet工作完毕后，将产生数据交给TwoServlet来使用
- Servlet规范中提供四种数据共享方案
  1. ServletContext接口
  2. Cookie类
  3. HttpSession接口
  4. HttpServletRequest接口

##### 1、ServletContext接口

1. 介绍：

   - 来自于Servlet规范中一个接口。在Tomcat中存在servlet-api.jar，在Tomcat中负责提供这个接口实现类
   - 如果两个Servlet来自于同一个网站。彼此之间通过网站的ServletContext实例对象实现数据共享
   - 开发人员习惯于将ServletContext对象称为【全局作用域对象】

2.  工作原理：

   ​	每一个网站都存在一个全局作用域对象。这个全局作用域对象【相当于】一个Map，在这个网站中OneServlet可以将一个数据存入到全局作用域对象，当前网站中其他Servlet此时都可以从全局作用域对象得到这个数据进行使用

   ![image-20211219193510826](03Servlet规范.assets\image-20211219193510826.png)

3.  全局作用域对象生命周期

   - 在Http服务器启动过程中，自动为当前网站在内存中创建一个全局作用域对象
   - 在Http服务器运行期间时，一个网站只有一个全局作用域对象
   - 在Http服务器运行期间，全局作用域对象一直处于存活状态
   - 在Http服务器准备关闭时，负责将当前网站中全局作用域对象进行销毁处理
   - 全局作用域对象生命周期贯穿网站整个运行期间

4.  命令实现： 【同一个网站】OneServlet将数据共享给TwoServlet

   ```java
   OneServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse response){
   		//1.通过【请求对象】向Tomcat索要当前网站中【全局作用域对象】
   		ServletContext application = request.getServletContext();
            //2.将数据添加到全局作用域对象作为【共享数据】
   		application.setAttribute("key1",数据);
   	}
   }
   
   TwoServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse response){
   		//1.通过【请求对象】向Tomcat索要当前网站中【全局作用域对象】
   		ServletContext application = request.getServletContext();
   		//2.从全局作用域对象得到指定关键字对应数据
   		Object 数据 =  application.getAttribute("key1");
   	}
   }
   ```

##### 2、Cookie

1. 介绍：

   - Cookie来自于Servlet规范中一个工具类，存在于Tomcat提供servlet-api.jar中
   - 如果两个Servlet来自于同一个网站，并且为同一个浏览器/用户提供服务，此时借助于Cookie对象进行数据共享
   - Cookie存放当前用户的私人数据，在共享数据过程中提高服务质量
   - 在现实生活场景中，Cookie相当于用户在服务端得到【会员卡】

2. 工作原理：

    	- 用户通过浏览器第一次向myWeb网站发送请求申请OneServlet。OneServlet在运行期间创建一个Cookie存储与当前用户相关数据，OneServlet工作完毕后，【将Cookie写入到响应头】交还给当前浏览器。
   - 浏览器收到响应响应包之后，将cookie存储在浏览器的缓存，一段时间之后，用户通过【**同一个浏览器**】再次向【**myWeb网站**】发送请求申请TwoServlet时。【浏览器需要无条件的将myWeb网站之前推送过来的Cookie，写入到请求头】发送过去，此时TwoServlet在运行时，就可以通过读取请求头中cookie中信息，得到OneServlet提供的共享数据。
   
   ![image-20211225153020922](03Servlet规范.assets\image-20211225153020922.png)
   
3. 实现命令 

   同一个网站 OneServlet 与  TwoServlet 借助于Cookie实现数据共享

   ```java
   OneServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse resp){
   		//1.创建一个cookie对象，保存共享数据（当前用户数据）
   		Cookie card = new Cookie("key1","abc");
   		Cookie card1= new Cookie("key2","efg");
   		//****cookie相当于一个map
   		//****一个cookie中只能存放一个键值对
   		//****这个键值对的key与value只能是String
   		//****键值对中key不能是中文
   		//2.【发卡】将cookie写入到响应头，交给浏览器
   		resp.addCookie(card);
   		resp.addCookie(card1)
   	}
   }
   ```

   ```java
   浏览器/用户      <---------响应包 【200】
   			                    【cookie: key1=abc; key2=eft】
   							   【】
   							   【处理结果】
   
   浏览器向myWeb网站发送请求访问TwoServlet---->请求包 【url:/myWeb/two method:get】
   			                                   【
   									          	请求参数: xxxx
   												Cookie: key1=abc;key2=efg
   									          】
   									         【】
   									         【】
                                                       
   TwoServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse resp){
   		//1.调用请求对象从请求头得到浏览器返回的Cookie
            Cookie cookieArray[] = request.getCookies();
           //2.循环遍历数据得到每一个cookie的key 与 value
   		for(Cookie card:cookieArray){
   			String key = card.getName(); //读取key  "key1"
   			Strign value = card.getValue();//读取value "abc"
   			//提供较好的服务。。。。。。。。
   		}
   	}
   }
   ```

4. 应用实例_订餐会员卡

   ![image-20211225161218775](03Servlet规范.assets\image-20211225161218775.png)

   ```java
   public class OneServlet extends HttpServlet {//别名：myWeb/one
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //1、调用请求对象读取【请求头】参数信息
           String userName = request.getParameter("userName");
           String money = request.getParameter("money");
           //2、开卡
           Cookie card1 = new Cookie("userName", userName);
           Cookie card2 = new Cookie("money", money);
           //3、发卡，将cookie写入到【响应头】中交给浏览器
           response.addCookie(card1);
           response.addCookie(card2);
           //4、通知Tomcat将【点餐页面】内容写入到【响应体】交给浏览器
           request.getRequestDispatcher("/index_2.html").forward(request, response);
       }
   }
   ```

   ```java
   public class TwoServlet extends HttpServlet {//别名：myWeb/two
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           int jiaozi_money = 30;
           int gaifan_money = 15;
           int miantiiao_money = 20;
           String userName = null;
           int money = 0, xiaofei = 0, balance = 0;
           Cookie newCard = null;
           response.setContentType("text/html;charset=utf-8");
           PrintWriter out = response.getWriter();
           //1、读取请求头参数信息，得到用户点餐食物类型
           String food = request.getParameter("food");
           //2、读取请求中Cookie
           Cookie[] cookieArray = request.getCookies();
           //3、消费刷卡
           for (Cookie card : cookieArray){
               String key = card.getName();
               String value = card.getValue();
               if("userName".equals(key)){
                   userName = value;
               }else if("money".equals(key)){
                   money = Integer.valueOf(value);
                   if("饺子".equals(food)){
                       if(jiaozi_money > money){
                           out.print("用户" + userName + "余额不足，请充值。");
                       } else {
                           newCard = new Cookie("money", (money - jiaozi_money) + "");
                           xiaofei = jiaozi_money;
                           balance = money - jiaozi_money;
                       }
                   } else if("面条".equals(food)){
                       if(miantiiao_money > money){
                           out.print("用户" + userName + "余额不足，请充值。");
                       } else {
                           newCard = new Cookie("money", (money - miantiiao_money) + "");
                           xiaofei = miantiiao_money;
                           balance = money - miantiiao_money;
                       }
                   } else if("盖饭".equals(food)){
                       if(gaifan_money > money){
                           out.print("用户" + userName + "余额不足，请充值。");
                       } else {
                           newCard = new Cookie("money", (money - gaifan_money) + "");
                           xiaofei = gaifan_money;
                           balance = money - gaifan_money;
                       }
                   }
               }
           }
           //4、将用户会员卡返还给用户
           response.addCookie(newCard);
           //5、将消费记录写入到响应体
           out.print("用户" + userName + "本次消费" + xiaofei + "元，余额 : " + balance);
       }
   }
   ```

   ```html
   <!--index.html-->
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
     <center>
       <font style="color: red;font-size: 40px">新会员申请开卡</font>
       <form action="/myWeb/one" method="get">
         <table border="2">
           <tr>
             <td>用户名</td>
             <td><input type="text" name="userName" /></td>
           </tr>
           <tr>
             <td>预存金额</td>
             <td><input type="text" name="money" /></td>
           </tr>
           <tr>
             <td><input type="submit" value="申请开卡" /></td>
             <td><input type="reset" /></td>
           </tr>
         </table>
       </form>
     </center>
   </body>
   </html>
   ```

   ```html
   <!--index_2.html-->
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
     <center>
       <font style="color:red;font-size: 40px">点餐页面</font>
       <form action="/myWeb/two">
         食物类型：
         <input type="radio" name="food" value="饺子">饺子（30元）
         <input type="radio" name="food" value="面条">面条（20元）
         <input type="radio" name="food" value="盖饭">盖饭（15元）<br>
         <input type="submit" value="划卡消费">
       </form>
     </center>
   </body>
   </html>
   ```

   ![image-20211225170343232](03Servlet规范.assets\image-20211225170343232.png)

   ![image-20211225170400083](03Servlet规范.assets\image-20211225170400083.png)

   ![image-20211225170406166](03Servlet规范.assets\image-20211225170406166.png)

5. Cookie生命周期

   - 在默认情况下，Cookie对象存放在浏览器的缓存中。因此只要浏览器关闭，Cookie对象就被销毁掉

   - 在手动设置情况下，可以要求浏览器将接收的Cookie存放在客户端计算机上硬盘上，同时需要指定Cookie在硬盘上存活时间。在存活时间范围内，关闭浏览器、关闭客户端计算机、关闭服务器都不会导致Cookie被销毁。在存活时间到达时，Cookie自动从硬盘上被删除

     ```java
     cookie.setMaxAge(60); //cookie在硬盘上存活1分钟
     ```

##### 3、HttpSession接口

1. 介绍

   -  HttpSession接口来自于Servlet规范下一个接口。存在于Tomcat中servlet-api.jar，其实现类由Http服务器提供。Tomcat提供实现类存在于servlet-api.jar
   - 如果两个Servlet来自于同一个网站，并且为同一个浏览器/用户提供服务，此时借助于HttpSession对象进行数据共享
   - 开发人员习惯于将HttpSession接口修饰对象称为【会话作用域对象】

2. HttpSession 与 Cookie 区别

   - 存储位置:  一个在天上，一个在地下

     ​	Cookie：存放在客户端计算机（浏览器内存/硬盘）
     ​	

     ​	HttpSession：存放在服务端计算机内存

   - 数据类型：Cookie对象存储共享数据类型只能是String；HttpSession对象可以存储任意类型的共享数据Object
     
   - 数据数量：一个Cookie对象只能存储一个共享数据；HttpSession使用map集合存储共享数据，所以可以存储任意数量共享数据
     
   - 参照物：Cookie相当于客户在服务端【会员卡】
     
     ​              HttpSession相当于客户在服务端【私人保险柜】

3. 命令实现：同一个网站（myWeb）下OneServlet将数据传递给TwoServlet

   ```java
   OneServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse response){
   		//1.调用请求对象向Tomcat索要当前用户在服务端的私人储物柜
   		HttpSession session = request.getSession();
            //2.将数据添加到用户私人储物柜
   		session.setAttribute("key1",共享数据)
   	}
   }
   ```

   ```java
   //浏览器访问/myWeb中TwoServlet
   TwoServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse response){
   		//1.调用请求对象向Tomcat索要当前用户在服务端的私人储物柜
   		HttpSession session = request.getSession();
            //2.从会话作用域对象得到OneServlet提供的共享数据
   		Object 共享数据 = session.getAttribute("key1");
   	}
   }
   ```

4.  应用实例_模拟购物车功能

   ![image-20211225174756573](03Servlet规范.assets\image-20211225174756573.png)

   ```java
   public class OneServlet extends HttpServlet {//别名：/one
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //1.调用请求对象，读取请求头参数，得到用户选择商品名
           String goodsName = request.getParameter("goodsName");
           //2.调用请求对象，向Tomcat索要当前用户在服务端的私人储物柜
           HttpSession session = request.getSession();
           //session.setMaxInactiveInterval(5);
           //3.将用户选购商品添加到当前用户私人储物柜
           Integer goodsNum = (Integer)session.getAttribute(goodsName);
           if(goodsNum == null){
               session.setAttribute(goodsName, 1);
           }else{
               session.setAttribute(goodsName, goodsNum+1);
           }
       }
   }
   ```

   ```java
   public class TwoServlet extends HttpServlet {//别名：/two
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //1.调用请求对象，向Tomcat索要当前用户在服务端私人储物柜
           HttpSession session = request.getSession();
           //2.将session中所有的key读取出来，存放一个枚举对象
           Enumeration goodsNames = session.getAttributeNames();
           while(goodsNames.hasMoreElements()){
               String goodsName =(String) goodsNames.nextElement();
               int goodsNum = (int) session.getAttribute(goodsName);
               System.out.println("商品名称 "+goodsName+" 商品数量 "+goodsNum);
           }
       }
   }
   ```

   ```html
   <!--index.html-->
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
     <table border="2" align="center">
       <tr>
         <td>商品名称</td>
         <td>商品单价</td>
         <td>供货商</td>
         <td>放入购物车</td>
       </tr>
       <tr>
         <td>华为笔记本pro13</td>
         <td>7000</td>
         <td>华为</td>
         <td><a href="/myWeb/one?goodsName=华为笔记本pro13">放入购物车</a></td>
       </tr>
       <tr>
         <td>罗技G304鼠标</td>
         <td>200</td>
         <td>罗技</td>
         <td><a href="/myWeb/one?goodsName=罗技G304鼠标">放入购物车</a></td>
       </tr>
       <tr>
         <td>ikbc机械键盘</td>
         <td>300</td>
         <td>ikbc</td>
         <td><a href="/myWeb/one?goodsName=ikbc机械键盘">放入购物车</a></td>
       </tr>
       <tr>
         <td colspan="4" align="center">
           <a href="/myWeb/two">查看我的购物车</a>
         </td>
       </tr>
     </table>
   </body>
   </html>
   ```

   ![image-20211225182035170](03Servlet规范.assets\image-20211225182035170.png)

5.  Http服务器如何将用户与HttpSession关联起来

   ![image-20211225183347098](03Servlet规范.assets\image-20211225183347098.png)

6. getSession()与getSession(false) 

   - getSession()：如果当前用户在服务端已经拥有了自己的私人储物柜，要求tomcat将这个私人储物柜进行返回；如果当前用户在服务端尚未拥有自己的私人储物柜，要求tocmat为当前用户创建一个全新的私人储物柜。
   - getSession(false)：如果当前用户在服务端已经拥有了自己的私人储物柜，要求tomcat将这个私人储物柜进行返回；如果当前用户在服务端尚未拥有自己的私人储物柜，此时Tomcat将返回null。

7. HttpSession生命周期

   - 用户与HttpSession关联时使用的Cookie只能存放在浏览器缓存中；
   - 在浏览器关闭时，意味着用户与他的HttpSession关系被切断；
   - 由于Tomcat无法检测浏览器何时关闭，因此在浏览器关闭时并不会导致Tomcat将浏览器关联的HttpSession进行销毁
   - 为了解决这个问题，Tomcat为每一个HttpSession对象设置【空闲时间】，这个空闲时间默认30分钟，如果当前HttpSession对象空闲时间达到30分钟，此时Tomcat认为用户已经放弃了自己的HttpSession，此时Tomcat就会销毁掉这个HttpSession

8. HttpSession空闲时间手动设置

   ```xml
   <!--在当前网站/web/WEB-INF/web.xml-->
   <session-config>
   	<session-timeout>5</session-timeout> <!--当前网站中每一个session最大空闲时间5分钟-->
   </session-config>
   ```

##### 4、HttpServletRequest接口

1. 介绍

   -   在同一个网站中，如果两个Servlet之间通过【请求转发】方式进行调用，彼此之间共享同一个请求协议包。而一个请求协议包只对应一个请求对象；因此servlet之间共享同一个请求对象，此时可以利用这个请求对象在两个Servlet之间实现数据共享
   - 在请求对象实现Servlet之间数据共享功能时，开发人员将请求对象称为【请求作用域对象】

2.  命令实现：OneServlet通过请求转发申请调用TwoServlet时，需要给TwoServlet提供共享数据

   ```java
   OneServlet{
   	public void doGet(HttpServletRequest req,HttpServletResponse response){
   		//1.将数据添加到【请求作用域对象】中attribute属性
   		req.setAttribute("key1",数据); //数据类型可以任意类型Object
            //2.向Tomcat申请调用TwoServlet
   		req.getRequestDispatcher("/two").forward(req,response)
   	}
   }
   ```

   ```java
   TwoServlet{
   	public void doGet(HttpServletRequest req,HttpServletResponse response){
           //从当前请求对象得到OneServlet写入到共享数据
   	    Object 数据 = req.getAttribute("key1");
   	}
   }
   ```

   ```java
   public class OneServlet extends HttpServlet {
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            //1.将数据添加到请求作用域对象，作为共享数据
             request.setAttribute("key1", "hello World");
            //2.代替浏览器，向Tomcat索要TwoServlet来完成剩余任务
             request.getRequestDispatcher("/two").forward(request, response);
       }
   }
   
   public class TwoServlet extends HttpServlet {
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            // 从同一个请求作用域对象得到OneServlet写入到共享数据
            String value =(String)request.getAttribute("key1");
            System.out.println("TwoServlet得到共享数据 "+value);
       }
   }
   ```

### 十三、监听器接口

1. 介绍  

   - 一组来自于Servlet规范下接口，共有8个接口。在Tomcat存在servlet-api.jar包
   - 监听器接口需要由开发人员亲自实现，Http服务器提供jar包并没有对应的实现类
   - 监听器接口用于监控【作用域对象生命周期变化时刻】以及【作用域对象共享数据变化时刻】

2.  作用域对象：

   -  在Servlet规范中，认为在服务端内存中可以在某些条件下为两个Servlet之间提供数据共享方案的对象，被称为【作用域对象】
   - Servlet规范下作用域对象:
     1. ServletContext：全局作用域对象
     2. HttpSession：会话作用域对象
     3. HttpServletRequest：请求作用域对象

3. 监听器接口实现类开发规范：三步

   - 根据监听的实际情况，选择对应监听器接口进行实现
   - 重写监听器接口声明【监听事件处理方法】
   - 在web.xml文件将监听器接口实现类注册到Http服务器

4.  ServletContextListener接口

   1. 作用：通过这个接口合法的检测全局作用域对象被初始化时刻以及被销毁时刻
   2. 监听事件处理方法：
      - public void contextInitlized(): 在全局作用域对象被Http服务器初始化被调用
      - public void contextDestory(): 在全局作用域对象被Http服务器销毁时候触发调用

   ```java
   package com.example.listener;
   
   import javax.servlet.ServletContextEvent;
   import javax.servlet.ServletContextListener;
   public class OneListener implements ServletContextListener {
       @Override
       public void contextInitialized(ServletContextEvent sce) {
           System.out.println("恭喜恭喜，来世走一遭！");
       }
       @Override
       public void contextDestroyed(ServletContextEvent sce) {
           System.out.println("兄弟莫怕，20年后又是一条好汉！");
       }
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       
       <!--将监听器接口实现类注册到Tomcat-->
       <listener>
           <listener-class>com.example.listener.OneListener</listener-class>
       </listener>
   </web-app>
   ```

5. ServletContextAttributeListener接口: 

   1. 作用：通过这个接口合法的检测全局作用域对象共享数据变化时刻
   2. 监听事件处理方法：
      - public void contextAdd(): 在全局作用域对象添加共享数据
      - public void contextReplaced(): 在全局作用域对象更新共享数据
      - public void contextRemove(): 在全局作用域对象删除共享数据

   ```java
   package com.example.listener;
   
   import javax.servlet.ServletContextAttributeEvent;
   import javax.servlet.ServletContextAttributeListener;
   
   public class OneListener implements ServletContextAttributeListener {
       @Override
       public void attributeAdded(ServletContextAttributeEvent scae) {
           System.out.println("新增共享数据");
       }
       @Override
       public void attributeRemoved(ServletContextAttributeEvent scae) {
           System.out.println("删除共享数据");
       }
       @Override
       public void attributeReplaced(ServletContextAttributeEvent scae) {
           System.out.println("更新共享数据");
       }
   }
   ```

   ```java
   package com.example.controller;
   
   import javax.servlet.ServletContext;
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   public class OneServlet extends HttpServlet {
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           ServletContext application = request.getServletContext();
           application.setAttribute("key1",100); //新增共享数据
           application.setAttribute("key1",200); //更新共享数据
           application.removeAttribute("key1");  //删除共享数据
       }
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
         <!--注册监听器接口实现类-->
         <listener>
             <listener-class>com.example.listener.OneListener</listener-class>
         </listener>
       <servlet>
           <servlet-name>OneServlet</servlet-name>
           <servlet-class>com.example.controller.OneServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>OneServlet</servlet-name>
           <url-pattern>/one</url-pattern>
       </servlet-mapping>
   </web-app> 
   ```

6. 全局作用域对象共享数据变化时刻 

   ```java
   ServletContext application = request.getServletContext();
   application.setAttribute("key1",100); //新增共享数据
   application.setAttribute("key1",200); //更新共享数据
   application.removeAttribute("key1");  //删除共享数据
   ```

7. 在线考试管理系统通过监听器优化

   原因：JDBC规范中，Connection创建与销毁最浪费时间

   解决方案：通过全局作用域对象得到Connection

   ```java
   package com.example.listener;
   
   import com.example.util.JdbcUtil;
   
   import javax.servlet.ServletContext;
   import javax.servlet.ServletContextEvent;
   import javax.servlet.ServletContextListener;
   import java.sql.Connection;
   import java.sql.SQLException;
   import java.util.HashMap;
   import java.util.Iterator;
   import java.util.Map;
   
   public class OneListener implements ServletContextListener {
       @Override
       public void contextInitialized(ServletContextEvent sce) {
           //在Tomcat启动时，预先创建20个Connection，在UserDao.add方法执行时
           //将实现创建好connection交给add方法
           Map<Connection, Boolean> map = new HashMap();
           for(int i = 0; i < 20; i++){
               try {
                   Connection conn = JdbcUtil.getConnection();
                   System.out.println("在Http服务器启动时，创建Connection" + conn);
                   map.put(conn, true);//true表示这个通道处于空闲状态，false表示通道正在被使用
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
           //为了在Http服务器运行期间，一直都可以使用20个Connection，将Connection保存
           //到全局作用域对象当中
           ServletContext servletContext = sce.getServletContext();
           servletContext.setAttribute("connections", map);
       }//map被销毁
   
       @Override
       public void contextDestroyed(ServletContextEvent sce) {
           //在Http服务器关闭时刻，将全局作用域对象中20个Connection对象销毁
           ServletContext servletContext = sce.getServletContext();
           Map<Connection,Boolean> map = (Map<Connection,Boolean>) servletContext.getAttribute("connections");
           Iterator<Connection> it = map.keySet().iterator();
           while (it.hasNext()){
               Connection conn = it.next();
               if(conn != null){
                   try {
                       conn.close();
                       System.out.println(conn + "已销毁！");
                   } catch (SQLException e) {
                       e.printStackTrace();
                   }
               }
           }
       }
   }
   ```

   ```java
   //JdbcUtil.java
   //原则：开闭原则
   //通过方法重载来写新的方法
   
   	/**
        * 通过全局作用域对象得到Connection
        * @param request
        * @return
        */
       public static Connection getConnection(HttpServletRequest request) throws SQLException {
           //1、通过请求对象，得到全局作用域对象
           ServletContext application = request.getServletContext();
           //2、从全局作用域对象得到map
           Map<Connection,Boolean> map = (Map<Connection,Boolean>) application.getAttribute("connections");
           //3、从Map得到一个处于空闲状态的Connection
           Iterator<Connection> it = map.keySet().iterator();
           Connection conn = null;
           while(it.hasNext()){
               conn = it.next();
               if(map.get(conn)){
                   map.put(conn,false);
                   break;
               }
           }
           return conn;
       }
   
    	/**
        * 关闭资源
        * @param conn 连接对象
        * @param ps   数据库操作对象
        * @param rs   结果集
        */
       public static void close(HttpServletRequest request, Connection conn, Statement ps, ResultSet rs){
           if(rs != null){
               try {
                   rs.close();
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
           if(ps != null){
               try {
                   ps.close();
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
           if(conn != null){
               ServletContext application = request.getServletContext();
               Map<Connection,Boolean> map = (Map<Connection, Boolean>) application.getAttribute("connections");
               map.put(conn,true);//将Connection通道重新设为空闲状态
           }
       }
   ```

   ```java
   //UserDao.java
   //原则：开闭原则
   //通过方法重载来写新的方法
   
   	/**
        * 用户注册,全局作用域对象
        * @return 1---注册成功，0---注册失败
        */
       public int add(Users user, HttpServletRequest request){
           //JDBC规范中，Connection创建与销毁最浪费时间
           Connection connection = null;
           PreparedStatement ps = null;
           int result = 0;
           try {
               connection = JdbcUtil.getConnection(request);
               String sql = "insert into users(userName,password,sex,email)" +
                       "values(?,?,?,?)";
               ps = connection.prepareStatement(sql);
               ps.setString(1, user.getUserName());
               ps.setString(2, user.getPassWord());
               ps.setString(3, user.getSex());
               ps.setString(4, user.getEmail());
               result = ps.executeUpdate();
           } catch (SQLException e) {
               e.printStackTrace();
           }finally {
               JdbcUtil.close(request, connection, ps, null);
           }
           return result;
       }
   .......
   ```

   ```xml
   <!--web.xml-->	
   	<!--注册监听器-->
       <listener>
           <listener-class>com.example.listener.OneListener</listener-class>
       </listener>
   ```

### 十四、Filter (过滤器接口)

1. 介绍

   - 来自于Servlet规范下接口，在Tomcat中存在于servlet-api.jar包
   - Filter接口实现类由开发人员负责提供，Http服务器不负责提供
   - Filter接口在Http服务器调用资源文件之前，对Http服务器进行拦截

2.  具体作用

   - 拦截Http服务器，帮助Http服务器检测当前请求合法性
   - 拦截Http服务器，对当前请求进行增强操作

3. Filter接口实现类开发步骤：三步

   - 创建一个Java类实现Filter接口
   - 重写Filter接口中doFilter()方法
   - web.xml将过滤器接口实现类注册到Http服务器

   ```java
   package com.example.filter;
   
   import javax.servlet.*;
   import java.io.IOException;
   import java.io.PrintWriter;
   
   public class OneFilter implements Filter {
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           //1.通过拦截请求对象得到请求包参数信息，从而得到来访用户的真实年龄
           String age = servletRequest.getParameter("age");
           //2.根据年龄，帮助Http服务器判断本次请求合法性
           if(Integer.valueOf(age) < 70){ //合法请求
               //将拦截请求对象和响应对象交还给Tomcat,由Tomcat继续调用资源文件
               filterChain.doFilter(servletRequest, servletResponse);//放行
           }else{
               //过滤器代替Http服务器拒绝本次请求
               servletResponse.setContentType("text/html;charset=utf-8");
               PrintWriter out = servletResponse.getWriter();
               out.print("<center><font style='color:red;font-size:40px'>大爷，珍爱生命啊!</font></center>");
           }
       }
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--将过滤器类文件路径交给Tomcat-->
       <filter>
           <filter-name>oneFilter</filter-name>
           <filter-class>com.example.filter.OneFilter</filter-class>
       </filter>
       <!--通知Tomcat在调用何种资源文件时需要被当前过滤器拦截-->
       <filter-mapping>
           <filter-name>oneFilter</filter-name>
           <url-pattern>/mm.jpg</url-pattern>
       </filter-mapping>
   </web-app>
   ```

   过滤器对请求对象进行增强服务

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
     <center>
       <form action="/myWeb/one" method="post">
         参数:<input type="text" name="userName" /><br/>
         <input type="submit" value="Post方式访问OneServlet">
       </form>
       <form action="/myWeb/two" method="post">
         参数:<input type="text" name="userName" /><br/>
         <input type="submit" value="Post方式访问TwoServlet">
       </form>
     </center>
   </body>
   </html>
   ```

   ```java
   package com.example.controller;
   
   import javax.servlet.*;
   import javax.servlet.http.*;
   import java.io.IOException;
   
   public class OneServlet extends HttpServlet {
       @Override
       protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //直接从请求体读取请求参数
           String userName = request.getParameter("userName");
           System.out.println("OneServlet 从请求体得到参数 " + userName);
       }
   }
   ```

   ```java
   package com.example.controller;
   
   import javax.servlet.*;
   import javax.servlet.http.*;
   import java.io.IOException;
   
   public class TwoServlet extends HttpServlet {
       @Override
       protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //直接从请求体读取请求参数
           String userName = request.getParameter("userName");
           System.out.println("TwoServlet 从请求体得到参数 " + userName);
       }
   }
   ```

   ```java
   package com.example.filter;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   public class OneFilter implements Filter {
       //通知拦截的请求对象，使用UTF-8字符集对当前请求体信息进行一次重新编辑
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           servletRequest.setCharacterEncoding("utf-8");//增强
           filterChain.doFilter(servletRequest, servletResponse);
       }
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>OneServlet</servlet-name>
           <servlet-class>com.example.controller.OneServlet</servlet-class>
       </servlet>
       <servlet>
           <servlet-name>TwoServlet</servlet-name>
           <servlet-class>com.example.controller.TwoServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>OneServlet</servlet-name>
           <url-pattern>/one</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>TwoServlet</servlet-name>
           <url-pattern>/two</url-pattern>
       </servlet-mapping>
   
       <filter>
           <filter-name>oneFilter</filter-name>
           <filter-class>com.example.filter.OneFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>oneFilter</filter-name>
           <url-pattern>/*</url-pattern><!--通知Tomcat在调用所有资源文件之前都需要调用OneFilter进行拦截-->
       </filter-mapping>
   </web-app>
   ```

4. Filter拦截地址格式

- 命令格式:

  ```xml
  <filter-mapping>
  	<filter-name>oneFilter</filter-name>
  	<url-pattern>拦截地址</url-pattern>
  </filter-mapping>
  ```

- 命令作用：拦截地址通知Tomcat在调用何种资源文件之前需要调用OneFilter过滤进行拦截

- 要求Tomcat在调用某一个具体文件之前，来调用OneFilter拦截

  ```xml
   <url-pattern>/img/mm.jpg</url-pattern>
  ```

- 要求Tomcat在调用某一个文件夹下所有的资源文件之前，来调用OneFilter拦截

  ```xml
  <url-pattern>/img/*</url-pattern>
  ```

- 要求Tomcat在调用任意文件夹下某种类型文件之前，来调用OneFilter拦截 

  ```xml
  <url-pattern>*.jpg</url-pattern>
  ```

- 要求Tomcat在调用网站中任意文件时，来调用OneFilter拦截

  ```xml
  <url-pattern>/*</url-pattern>
  ```


5. 在线考试管理系统_过滤器防止用户恶意登录行为

   ![image-20211226171837812](03Servlet规范.assets\image-20211226171837812.png)

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.调用请求对象对请求体使用utf-8字符集进行重新编辑
        request.setCharacterEncoding("utf-8");
        // 2.调用请求对象读取请求体参数信息
        String userName = request.getParameter("userName");
        String password = request.getParameter("password");
        // 3.调用UserDao将查询验证信息推送到数据库服务器上
        UserDao userDao = new UserDao();
        int result = userDao.ligin(userName, password, request);
        //4.调用响应对象，根据验证结果将不同资源文件地址写入到响应头，交给浏览器
        if(result == 1){
            //在判定来访用户身份合法后，通过请求对象向Tomcat申请为当前用户申请一个HttpSession
            HttpSession session = request.getSession();
            response.sendRedirect("/myWeb/index.html");
        } else {
            response.sendRedirect("/myWeb/login_error.html");
        }
    }
}
```

```java
public class UserFindServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //索要当前用户在服务端HttpSession
        HttpSession session = request.getSession(false);
        if(session == null){
            response.sendRedirect("/myWeb/login_error.html");
            return;
        }

        //提供服务
        //1、[调用UserDao] 将查询命令推送到数据库服务器上，得到所有用户信息【List】
        UserDao userDao = new UserDao();
        List<Users> allUsers = userDao.findAll(request);
        //2、[调用响应对象] 将用户信息结合<table>标签命令以二进制形式写入到响应体
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("<table border='2' align='center' >");
        out.print("<tr>");
        out.print("<td>用户编号</td>");
        out.print("<td>用户姓名</td>");
        out.print("<td>用户密码</td>");
        out.print("<td>用户性别</td>");
        out.print("<td>用户邮箱</td>");
        out.print("<td>操作</td>");
        out.print("</tr>");
        for(Users user : allUsers){
            out.print("<tr>");
            out.print("<td>" + user.getUserId() +"</td>");
            out.print("<td>" + user.getUserName() +"</td>");
            out.print("<td>******</td>");
            out.print("<td>" + user.getSex() +"</td>");
            out.print("<td>" + user.getEmail() +"</td>");
            out.print("<td><a href='/myWeb/user/delete?userId=" + user.getUserId() + "' >删除用户</a></td>");
            out.print("</tr>");
        }
        out.print("</table>");
    }
}
```

![image-20211226173218042](03Servlet规范.assets\image-20211226173218042.png)

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.调用请求对象对请求体使用utf-8字符集进行重新编辑
        request.setCharacterEncoding("utf-8");
        // 2.调用请求对象读取请求体参数信息
        String userName = request.getParameter("userName");
        String password = request.getParameter("password");
        // 3.调用UserDao将查询验证信息推送到数据库服务器上
        UserDao userDao = new UserDao();
        int result = userDao.ligin(userName, password, request);
        //4.调用响应对象，根据验证结果将不同资源文件地址写入到响应头，交给浏览器
        if(result == 1){
            //在判定来访用户身份合法后，通过请求对象向Tomcat申请为当前用户申请一个HttpSession
            HttpSession session = request.getSession();
            response.sendRedirect("/myWeb/index.html");
        } else {
            response.sendRedirect("/myWeb/login_error.html");
        }
    }
}
```

```java
package com.example.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class OneFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        //1.调用请求对象读取请求包中请求行URI，了解用户访问的资源文件是谁
        String uri = request.getRequestURI();//[/网站名/资源文件名 /myWeb/login.html 或 /myWeb/login 或 ...]
        //2.如果本次请求资源文件与登录相关【login.html 或者 LoginServlet】 此时应该无条件放行
        // /myWeb/ 表示初始打开Tomcat时传进来的参数
        if(uri.indexOf("login") != -1 || "/myWeb/".equals(uri)){
            filterChain.doFilter(servletRequest, servletResponse);
            return;
        }
        //3.如果本次请求访问的是其他资源文件，需要得到用户在服务端的HttpSession
        HttpSession session = request.getSession(false);
        if(session != null){
            filterChain.doFilter(servletRequest, servletResponse);
            return;
        }
        //4.拒绝请求
        request.getRequestDispatcher("/login_error.html").forward(servletRequest, servletResponse);
    }
}
```

```xml
	<!--注册过滤器-->
    <filter>
        <filter-name>oneFilter</filter-name>
        <filter-class>com.example.filter.OneFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>oneFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 十五、第三版互联网通信流程图

![image-20211226182822915](03Servlet规范.assets\image-20211226182822915.png)
# JSP

### 一、JSP规范介绍

- 来自于JAVAEE规范中一种
- JSP规范制定了如何开发JSP文件代替响应对象将处理结果写入到响应体的开发流程
- JSP规范制定了Http服务器应该如何调用管理JSP文件

### 二、响应对象存在弊端

- 适合将数据量较少的处理结果写入到响应体
- 如果处理结果数量过多，使用响应对象增加开发难度

### 三、JSP文件优势

- JSP文件在互联网通信过程，是响应对象替代品.
- 降低将处理结果写入到响应体的开发工作量降低处理结果维护难度
- 在JSP文件开发时，可以直接将处理结果写入到JSP文件不需要手写out.print()命令，在Http服务器调用JSP文件时，根据JSP规范要求自动的将JSP文件书写的所有内容通过输出流写入到响应体

### 四、HTML文件与JSP文件区别

- 作为资源文件类型不同
  - HTML文件属于静态资源文件，其相关命令需要在浏览器编译并执行的.
  - JSP文件属于动态资源文件，其相关命令需要在服务端编译并执行的
- 调用形式不同
  - 如果浏览器访问HTML文件，此时Http服务器直接通过一个输出流将HTML文件中所有的内容写入到响应体
  - 如果浏览器访问JSP文件。此时Http服务器根据JSP规范来操作JSP文件编辑---->编译----->调用

### 五、JSP文件运行原理

```
一。Http服务器调用JSP文件步骤:
    1.Http服务器将JSP文件内容【编辑】为一个Servlet接口实现类（.java）
    2.Http服务器将Servlet接口实现类【编译】为class文件(.class)
    3.Http服务器负责创建这个class的实例对象，这个实例对象就是Servlet实例对象
    4.Http服务器通过Servlet实例对象调用_jspService方法，将jsp文件内容写入到响应体
二。Http服务器【编辑】与【编译】JSP文件位置：
    标准答案：我在【work】下看到这个证据
    C:\Users\[登录windows系统用户角色名]\.IntelliJIdea2018.3\system\tomcat\[网站工作空间]\work\Catalina\localhost\【网站别名】\org\apache\jsp
```

- Tomcat根据JSP规范，将被访问的JSP文件[编辑]为一个java文件。这个Java文件是Servlet接口实现类
- Tomcat根据JSP规范，调用JVM（javac one_jsp.java）将这个java文件[编译]为class类型
- Tomcat根据JSP规范负责生成这个class文件的实例对象。这个实例对象是一个Servelt接口实例对象
- Tomcat根据JSP规范通过实例对象调用class文件中_jspService方法
- _jspService方法在运行时负责将JSP文件中书写内容写入到响应体中

### 六、在JSP文件中如何书写Java命令

1. 执行标记命令格式：

```jsp
<% int a  =10;  %> <!--声明局部变量-->
<% boolean flag = 30 >= 40; %>  <!--Java中表达式(数学表达式，关系表达式，逻辑表达式)-->
<%
	if(判断条件){				    
	}else{
	}
	while(){
	}
%>  <!--书写控制语句-->
```

2. 执行标记命令作用：通知Http服务器将JSP文件中Java命令与其他普通执行结果进行区分

3. 输出标记命令格式

   ```jsp
   <%=java的变量名%>
   <%=java的表达式%>
   ```

4. 输出标记命令作用：通知Tomcat将输出标记中【变量的值】或则输出标记中【表达式运算结果】写入到响应体

5. 代码：

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <!--在JSP文件中直接书写Java命令，是不能被JSP文件识别，此时只会被当做字符串写入到响应体-->
   int num1 =20;
   int num2 =30;
   int num3 = num1 + num2;
   ```

   ![image-20211226200012886](04JSP_EL_MVC_AJAX_JSON.assets\image-20211226200012886.png)

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%
     //在jsp文件中，只有书写在执行标记中内容才会被当做Java命令
     //1.声明Java变量
     int num1 = 100;
     int num2 = 200;
     //2.声明运行表达式：数学运算，关系运算，逻辑运算
     int num3 = num1 + num2; //数学运算
     int num4 = num2>=num1?num2:num1;//关系运算
     boolean num5 = num2>=200 && num1>=100;//逻辑运算
     //3.声明控制语句
     if(num2>=num1){
     }else{
     }
     for(int i=1;i<=10;i++){
     }
   %>
   ```

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%
      int num1 =100;
      int num2 =200;
   %>
   <!--在JSP文件，通过输出标记，通知JSP将Java变量的值写入到响应体-->
   变量num1的值:<%=num1%><br/>
   变量num2的值:<%=num2%><br/>
   <!--执行标记还可以通知Jsp将运算结果写入到响应体-->
   num1 + num2 = <%=num1+num2%>
   ```

   ![image-20211226194432876](04JSP_EL_MVC_AJAX_JSON.assets\image-20211226194432876.png)

   ```jsp
   <%@ page import="com.example.entity.Student" %><!--自动导包-->
   <%@ page import="java.util.*" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%
       //创建一个Student类型对象
       Student stu = new Student(10,"mike");//id name
       List list= new ArrayList();
   %>
   学员编号:<%=stu.getSid()%><br/>
   学员姓名:<%=stu.getSname()%>
   ```

   ![image-20211226194744250](04JSP_EL_MVC_AJAX_JSON.assets\image-20211226194744250.png)

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <!--JSP中所有的执行标记当作一个整体-->
   <%
     int age =15;
   %>
   
   <%
     if(age >=18){
   %>
   
   <font style="color:red;font-size:45px">热烈欢迎</font>
   
   <%
     }else{
   %>
   
   <font style="color:red;font-size:45px">谢绝入内</font>
   
   <%
     }
   %>
   
   <!--
   {
   	int age =15;
   	if(age>=18){
   		out.print(<font style="color:red;font-size:45px">热烈欢迎</font>)
   	}else{
   		out.print(<font style="color:red;font-size:45px">热烈欢迎</font>)
   	}
   }
   -->
   ```

   ```jsp
   <%@ page import="com.example.entity.Student" %>
   <%@ page import="java.util.List" %>
   <%@ page import="java.util.ArrayList" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <!--制造数据-->
   <%
       Student stu1 = new Student(10,"mike");
       Student stu2 = new Student(20,"allen");
       Student stu3 = new Student(30,"smith");
       List<Student> list = new ArrayList();
       list.add(stu1);
       list.add(stu2);
       list.add(stu3);
   %>
   
   <!--数据输出-->
   <table border="2" align="center">
       <tr>
           <td>学员编号</td>
           <td>学员姓名</td>
       </tr>
   <%
       for(Student stu:list){
   %>
       <tr>
           <td><%=stu.getSid()%></td>
           <td><%=stu.getSname()%></td>
       </tr>
   <%
       }
   %>
   </table>
   ```

   ![image-20211226200033995](04JSP_EL_MVC_AJAX_JSON.assets\image-20211226200033995.png)

### 七、JSP文件内置对象

1. request

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--
    JSP文件内置对象: request
        类型：HttpServletRequest
        作用: 在JSP文件运行时读取请求包信息
            与Servlet在请求转发过程中实现数据共享
    浏览器： http://localhost:8080/myWeb/request.jsp?userName=allen&password=123
-->
<%
    //在JSP文件执行时，借助于内置request对象读取请求包参数信息
    String userName = request.getParameter("userName");
    String password =request.getParameter("password");
%>
来访用户姓名:<%=userName%><br/>
来访用户密码:<%=password%>
```

![image-20211226200647604](04JSP_EL_MVC_AJAX_JSON.assets\image-20211226200647604.png)

2. session

```jsp
session.jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--
    JSP文件内置对象:session
      类型:HttpSession
      作用：JSP文件在运行时，可以session指向当前用户私人储物柜，添加
          共享数据，或则读取共享数据
-->
<!--将共享数据添加到当前用户私人储物柜-->
<%
  // HttpSession session = request.getSession();
  session.setAttribute("key1", 200);
%>
```

```jsp
session2.jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--
    session.jsp 与  session2.jsp为同一个用户/浏览器提供服务。
    因此可以使用当前用户在服务端的私人储物柜进行数据共享
-->
<%
    Integer value=(Integer) session.getAttribute("key1");
%>
session_2.jsp从当前用户session中读取数据:<%=value%>
```

![image-20211226201214658](04JSP_EL_MVC_AJAX_JSON.assets\image-20211226201214658.png)

3. application

```jsp
<!--
       ServletContext application;全局作用域对象
       同一个网站中Servlet与JSP，都可以通过当前网站的全局作用域对象实现数据共享
       JSP文件内置对象 ： application
-->

<%
    application.setAttribute("key1", "hello world");
%>
```

```java
public class OneServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          ServletContext application = request.getServletContext();
          String value = (String)application.getAttribute("key1");
        System.out.println("value = "+value);
    }
}
```

### 八、Servlet与JSP文件分工

```
一。Servlet与JSP分工:
	Servlet： 负责处理业务并得到【处理结果】--------------------大厨
	JSP：     不负责业务处理，主要任务将Servlet中【处理结果】写入到响应体----传菜员
二。Servlet与JSP之间调用关系
	Servlet工作完毕后，一般通过请求转发方式向Tomcat申请调用JSP
三。Servlet与JSP之间如何实现数据共享
	Servlet将处理结果添加到【请求作用域对象】
	JSP文件在运行时从【请求作用域对象】得到处理结果
```

```java
public class Student {
    private Integer sid;
    private String sname;
    public Integer getSid() {
        return sid;
    }
    public void setSid(Integer sid) {
        this.sid = sid;
    }
    public String getSname() {
        return sname;
    }
    public void setSname(String sname) {
        this.sname = sname;
    }
    public Student() {
    }
    public Student(Integer sid, String sname) {
        this.sid = sid;
        this.sname = sname;
    }
}
```

```java
public class OneServlet extends HttpServlet {
    //处理业务，得到处理结果-----查询学员信息
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Student s1 = new Student(10,"mike");
        Student s2 = new Student(20,"allen");
        List<Student> stuList = new ArrayList();
        stuList.add(s1);
        stuList.add(s2);

        //将处理结果添加到请求作用域对象
        request.setAttribute("key", stuList);

        //通过请求转发方案，向Tomcat申请调用user_show.jsp
        //同时将request与response通过tomcat交给user_show.jsp使用
        request.getRequestDispatcher("/user_show.jsp").forward(request, response);
    }
}
```

```jsp
<%@ page import="java.util.List" %>
<%@ page import="com.example.entity.Student" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<%
    //从请求作用域对象得到OneServlet添加进去的集合
    List<Student> stuList = (List<Student>) request.getAttribute("key");
%>
<!--将处理结果写入到响应体-->
<table border="2" align="center">
    <tr>
        <td>学生编号</td>
        <td>学生姓名</td>
    </tr>
    <%
        for(Student student : stuList){
    %>
    <tr>
        <td><%=student.getSid()%></td>
        <td><%=student.getSname()%></td>
    </tr>
    <%
        }
    %>
</table>
```

![image-20220105191113932](04JSP_EL_MVC_AJAX_JSON.assets\image-20220105191113932.png)

### 九、在线考试管理系统---试题功能

```
准备工作
任务： 在线考试管理系统----试题信息管理模块
子任务:
       添加试题
       查询试题
       删除试题
       更新试题
准备工作：
       1.准备试题信息表:单选题，每道题有4个选项  一个正确答案
         create table question(
           questionId int primary key auto_increment,
           title  varchar(50),# 10-8=?
           optionA varchar(20),#A: 9
           optionB varchar(20),#B: 1
           optionC varchar(20),#C: 2
           optionD varchar(20),#D: 0
           answer  char(1)     #正确答案： C
         )
         2.com.bjpowernode.entity.Question
```

##### 1 试题添加功能

```java
public class Question {
    private Integer questionId;
    private String title;
    private String optionA;
    private String optionB;
    private String optionC;
    private String optionD;
    private String answer;
    public Question() {
    }
    public Question(Integer questionId, String title, String optionA, String optionB, String optionC, String optionD, String answer) {
        this.questionId = questionId;
        this.title = title;
        this.optionA = optionA;
        this.optionB = optionB;
        this.optionC = optionC;
        this.optionD = optionD;
        this.answer = answer;
    }

    public Integer getQuestionId() {
        return questionId;
    }
    public void setQuestionId(Integer questionId) {
        this.questionId = questionId;
    }

    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }

    public String getOptionA() {
        return optionA;
    }
    public void setOptionA(String optionA) {
        this.optionA = optionA;
    }

    public String getOptionB() {
        return optionB;
    }
    public void setOptionB(String optionB) {
        this.optionB = optionB;
    }

    public String getOptionC() {
        return optionC;
    }
    public void setOptionC(String optionC) {
        this.optionC = optionC;
    }

    public String getOptionD() {
        return optionD;
    }
    public void setOptionD(String optionD) {
        this.optionD = optionD;
    }

    public String getAnswer() {
        return answer;
    }
    public void setAnswer(String answer) {
        this.answer = answer;
    }
}
```

```java
public class QuestionDao {
    public static int add(Question question){
        Connection conn = null;
        PreparedStatement ps = null;
        int res = 0;
        try {
            String sql = "insert into question(title, optionA, optionB, optionC, optionD, answer) values(?,?,?,?,?,?)";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            ps.setString(1, question.getTitle());
            ps.setString(2, question.getOptionA());
            ps.setString(3, question.getOptionB());
            ps.setString(4, question.getOptionC());
            ps.setString(5, question.getOptionD());
            ps.setString(6, question.getAnswer());
            res = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, null);
        }
        return res;
    }
}
```

```java
public class QuestiomAddServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String title, optionA, optionB, optionC, optionD, answer;
        //1.调用请求对象读取请求信息，得到新增信息内容
        title = request.getParameter("title");
        optionA = request.getParameter("optionA");
        optionB = request.getParameter("optionB");
        optionC = request.getParameter("optionC");
        optionD = request.getParameter("optionD");
        answer = request.getParameter("answer");
        //2.调用Dao对象将Insert命令推送到数据库，并得到处理结果
        Question question = new Question(null, title, optionA, optionB, optionC, optionD, answer);
        int res = QuestionDao.add(question);
        //3.通过请求转发，向Tomcat索要info.jsp将处理结果写入到响应体
        if(res == 1){
            request.setAttribute("info", "试题添加成功");
        } else {
            request.setAttribute("info", "试题添加失败");
        }
        //4.请求转发向Tomcat申请info.jsp将结果写入到响应体
        request.getRequestDispatcher("/info.jsp").forward(request, response);
    }
}
```

```jsp
<!--info.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <center>
    <%
      String info = (String) request.getAttribute("info");
    %>
    <font style="color: red; font-size: 45px">
      <%=info%>
    </font>
  </center>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <Center>
    <form action="/myWeb/question/add" method="get">
      <table border="2">
        <tr>
          <td>题目:</td>
          <td><input type="text" name="title"></td>
        </tr>
        <tr>
          <td>A:</td>
          <td><input type="text" name="optionA"></td>
        </tr>
        <tr>
          <td>B:</td>
          <td><input type="text" name="optionB"></td>
        </tr>
        <tr>
          <td>C:</td>
          <td><input type="text" name="optionC"></td>
        </tr>
        <tr>
          <td>D:</td>
          <td><input type="text" name="optionD"></td>
        </tr>
        <tr>
          <td>正确答案:</td>
          <td>
            <input type="radio" name="answer" value="A">A
            <input type="radio" name="answer" value="B">B
            <input type="radio" name="answer" value="C">C
            <input type="radio" name="answer" value="D">D
          </td>
        </tr>
        <tr>
          <td><input type="submit" value="新增试题"></td>
          <td><input type="reset" ></td>
        </tr>
      </table>
    </form>
  </Center>
</body>
</html>
```

![image-20220105204156325](04JSP_EL_MVC_AJAX_JSON.assets\image-20220105204156325.png)

 ![image-20220105204213412](04JSP_EL_MVC_AJAX_JSON.assets\image-20220105204213412.png)

##### 2 所有试题查询功能

```java
public class QuestionFindServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.调用DAO对象将查询命令推送到数据库中得到试题信息【List】
        List<Question> questionList = QuestionDao.findAll();
        //2.通过请求转发，向Tomcat索要question.jsp，将试题信息写入到响应体
        request.setAttribute("key", questionList);
        request.getRequestDispatcher("/question.jsp").forward(request, response);
    }
}
```

```java
public class QuestionDao {
    public static List<Question> findAll(){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        List<Question> questionList = new ArrayList<>();
        try {
            String sql = "select * from question";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            rs = ps.executeQuery();
            while(rs.next()){
                Integer questionId = rs.getInt("questionId");
                String title = rs.getString("title");
                String optionA = rs.getString("optionA");
                String optionB = rs.getString("optionB");
                String optionC = rs.getString("optionC");
                String optionD = rs.getString("optionD");
                String answer = rs.getString("answer");
                Question question = new Question(questionId, title, optionA, optionB, optionC, optionD, answer);
                questionList.add(question);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return questionList;
    }
}
```

```jsp
<%@ page import="java.util.List" %>
<%@ page import="com.example.entity.Question" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <table border="2" align="center">
      <tr>
          <td>试题编号</td>
          <td>试题信息</td>
          <td>A</td>
          <td>B</td>
          <td>C</td>
          <td>D</td>
          <td>正确答案</td>
      </tr>
      <%
          List<Question> list = (List<Question>) request.getAttribute("key");
          for(int i = 0; i < list.size(); i++){
              Question question = list.get(i);
       %>
      <%
        if(i % 2 == 0){
       %>
      <tr style="background-color: green">
      <%
        }else{
      %>
      <tr style="background-color: yellow">
      <%
        }
      %>
          <td><%=question.getQuestionId()%></td>
          <td><%=question.getTitle()%></td>
          <td><%=question.getOptionA()%></td>
          <td><%=question.getOptionB()%></td>
          <td><%=question.getOptionC()%></td>
          <td><%=question.getOptionD()%></td>
          <td><%=question.getAnswer()%></td>
      </tr>
      <%
          }
      %>
  </table>
</body>
</html>
```

![image-20220106195801195](04JSP_EL_MVC_AJAX_JSON.assets\image-20220106195801195.png)

##### 3 试题删除功能

```java
public class QuestionDeleteServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.调用请求对象读取请求头参数信息，得到试题编号
        String questionId = request.getParameter("questionId");
        //2.调用Dao对象将删除命令推送到数据库服务器
        int result = QuestionDao.delete(questionId);
        //3.调用info.jsp将处理结果写入到响应体
        if(result ==1){
            request.setAttribute("info", "试题删除成功");
        }else{
            request.setAttribute("info", "试题删除失败");
        }
        request.getRequestDispatcher("/info.jsp").forward(request, response);
    }
}
```

```java
public class QuestionDao {
    public static int delete(String questionId){
        Connection conn = null;
        PreparedStatement ps = null;
        int res = 0;
        try {
            conn = JdbcUtil.getConnection();
            String sql="delete from question where questionId=?";
            ps = conn.prepareStatement(sql);
            ps.setInt(1, Integer.valueOf(questionId));
            res = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, null);
        }
        return res;
    }
}

```

```jsp
<%@ page import="java.util.List" %>
<%@ page import="com.example.entity.Question" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <table border="2" align="center">
      <tr>
          <td>试题编号</td>
          <td>试题信息</td>
          <td>A</td>
          <td>B</td>
          <td>C</td>
          <td>D</td>
          <td>正确答案</td>
      </tr>
      <%
          List<Question> list = (List<Question>) request.getAttribute("key");
          for(int i = 0; i < list.size(); i++){
              Question question = list.get(i);
       %>
      <%
        if(i % 2 == 0){
       %>
      <tr style="background-color: green">
      <%
        }else{
      %>
      <tr style="background-color: yellow">
      <%
        }
      %>
          <td><%=question.getQuestionId()%></td>
          <td><%=question.getTitle()%></td>
          <td><%=question.getOptionA()%></td>
          <td><%=question.getOptionB()%></td>
          <td><%=question.getOptionC()%></td>
          <td><%=question.getOptionD()%></td>
          <td><%=question.getAnswer()%></td>
          <td style="background-color: white">
              <a href="/myWeb/question/delete?questionId=<%=question.getQuestionId()%>">删除试题</a>
          </td>
          <td style="background-color: white">
              <a href="/myWeb/question/findById?questionId=<%=question.getQuestionId()%>">详细信息</a>
          </td>
      </tr>
      <%
          }
      %>
  </table>
</body>
</html>
```

![image-20220106201703921](04JSP_EL_MVC_AJAX_JSON.assets\image-20220106201703921.png)

#####  4 试题编号查询功能

```java
public class QuestionFindByIdServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String questionId;
        Question question = null;
        //1.调用请求对象读取请求头中参数信息，得到试题编号
        questionId = request.getParameter("questionId");
        //2.调用Dao推送查询命令得到这个试题编号对应的试题信息
        question = QuestionDao.findById(questionId);
        //3.调用question_update.jsp将试题信息写入到响应体
        request.setAttribute("key", question);
        request.getRequestDispatcher("/question_update.jsp").forward(request, response);
    }
}
```

```java
public class QuestionDao {
    public static Question findById(String questionId) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        Question question = null;
        try {
            String sql = "select * from question where questionId=?";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            ps.setInt(1, Integer.valueOf(questionId));
            rs = ps.executeQuery();
            while(rs.next()){
                Integer quesId = rs.getInt("questionId");
                String title = rs.getString("title");
                String optionA = rs.getString("optionA");
                String optionB = rs.getString("optionB");
                String optionC = rs.getString("optionC");
                String optionD = rs.getString("optionD");
                String answer = rs.getString("answer");
                question = new Question(quesId, title, optionA, optionB, optionC, optionD, answer);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return question;
    }
}
```

```jsp
<%@ page import="com.example.entity.Question" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
  <title>Title</title>
</head>
<body>
<%
  Question q =(Question) request.getAttribute("key");
%>
<center>
  <form action="/myWeb/question/update" method="get">
    <table border="2">
      <tr>
        <td>题目编号:</td>
        <td><input type="text" name="questionId" value="<%=q.getQuestionId()%>" readOnly></td>
      </tr>
      <tr>
        <td>题目:</td>
        <td><input type="text" name="title" value="<%=q.getTitle()%>"></td>
      </tr>
      <tr>
        <td>A:</td>
        <td><input type="text" name="optionA" value="<%=q.getOptionA()%>"></td>
      </tr>
      <tr>
        <td>B:</td>
        <td><input type="text" name="optionB" value="<%=q.getOptionB()%>"></td>
      </tr>
      <tr>
        <td>C:</td>
        <td><input type="text" name="optionC" value="<%=q.getOptionC()%>"></td>
      </tr>
      <tr>
        <td>D:</td>
        <td><input type="text" name="optionD" value="<%=q.getOptionD()%>"></td>
      </tr>
      <tr>
        <td>正确答案:</td>
        <td>
          <input type="radio" name="answer" value="A" <%="A".equals(q.getAnswer())?"checked":""%> >A
          <input type="radio" name="answer" value="B" <%="B".equals(q.getAnswer())?"checked":""%> >B
          <input type="radio" name="answer" value="C" <%="C".equals(q.getAnswer())?"checked":""%> >C
          <input type="radio" name="answer" value="D" <%="D".equals(q.getAnswer())?"checked":""%> >D
        </td>
      </tr>
      <tr>
        <td><input type="submit" value="更新试题"/></td>
        <td><input type="reset" ></td>
      </tr>
    </table>
  </form>
</center>
</body>
</html>
```

![image-20220106203638553](04JSP_EL_MVC_AJAX_JSON.assets\image-20220106203638553.png)

![image-20220106203641643](04JSP_EL_MVC_AJAX_JSON.assets\image-20220106203641643.png)

##### 5 试题更新功能

```java
public class QuestionUpdateServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String title,optionA,optionB,optionC,optionD,answer,questionId;
        Question question = null;
        int result = 0;
        //1.调用请求对象读取请求头参数信息
        title= request.getParameter("title");
        optionA= request.getParameter("optionA");
        optionB= request.getParameter("optionB");
        optionC = request.getParameter("optionC");
        optionD = request.getParameter("optionD");
        answer  = request.getParameter("answer");
        questionId = request.getParameter("questionId");
        //2.调用Dao实现更新操作
        question = new Question(Integer.valueOf(questionId), title, optionA, optionB, optionC, optionD, answer);
        result = QuestionDao.update(question);
        //3.调用info.jsp将操作结果写入到响应体
        if(result == 1){
            request.setAttribute("info", "试题更新成功");
        }else{
            request.setAttribute("info", "试题更新失败");
        }
        request.getRequestDispatcher("/info.jsp").forward(request, response);
    }
}
```

```java
public class QuestionDao {
    public static int update(Question question) {
        Connection conn = null;
        PreparedStatement ps = null;
        int res = 0;
        try {
            String sql = "update question set title=?, optionA=?, optionB=?, optionC=?, optionD=?, answer=? where questionId=?";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            ps.setString(1, question.getTitle());
            ps.setString(2, question.getOptionA());
            ps.setString(3, question.getOptionB());
            ps.setString(4, question.getOptionC());
            ps.setString(5, question.getOptionD());
            ps.setString(6, question.getAnswer());
            ps.setInt(7, question.getQuestionId());
            res = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, null);
        }
        return res;
    }
}
```

# EL表达式

### 一、EL 工具包介绍

- 由Java技术开发一个jar包
- 作用降低JSP文件开发时Java命令开发强度
- Tomcat服务器本身自带了EL工具包（Tomcat安装地址/lib/el-api.jar）

### 二、JSP文件开发步骤

- JSP文件作用：代替 响应对象 将 Servlet 中 doGet/doPost 的执行结果写入到响应体

- JSP开发步骤

  ```jsp
  <%
  	String value = (String) request.getAttribute("key");
  %>
  <%=value%>
  ```

  1. 从指定的作用域对象读取处理结果
  2. 将得到数据进行类型强转
  3. 将转换后的数据写入到响应体
  
  ```java
  public class OneServlet extends HttpServlet {//  /myWeb/one
      @Override
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          //1.分别将共享数据添加到作用域对象
          ServletContext application = request.getServletContext();
          HttpSession session = request.getSession();
  
          application.setAttribute("sid", 10);
          session.setAttribute("sname", "mike");
          request.setAttribute("home", "nanjing");
  
          //2.通过请求转发方式，向Tomcat申请调用index_1.jsp，由index_1.jsp负责将
          // 作用域对象共享数据读取并写入到响应体，交给浏览器
          request.getRequestDispatcher("/index_1.jsp").forward(request, response);
      }
  }
  ```
  
  ```jsp
  <!--index_1.jsp-->
  <%--
    Created by IntelliJ IDEA.
    User: Amadeus
    Date: 2022/1/8
    Time: 16:50
    To change this template use File | Settings | File Templates.
  --%>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <%
      Integer sid = (Integer) application.getAttribute("sid");
      String sname = (String) session.getAttribute("sname");
      String home = (String) request.getAttribute("home");
  %>
  学员ID:<%=sid%><br>
  学员姓名:<%=sname%><br>
  学员地址:<%=home%>
  ```
  
  ![image-20220108165806394](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108165806394.png)

### 三、EL表达式

```JSp
<%--
  Created by IntelliJ IDEA.
  User: Amadeus
  Date: 2022/1/8
  Time: 16:50
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    Integer sid = (Integer) application.getAttribute("sid");
    String sname = (String) session.getAttribute("sname");
    String home = (String) request.getAttribute("home");
%>
学员ID:<%=sid%><br>
学员姓名:<%=sname%><br>
学员地址:<%=home%>

<hr/>
学员ID:${applicationScope.sid}<br>
学员姓名:${sessionScope.sname}<br>
学员地址:${requestScope.home}<br>
```

![image-20220108170302739](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108170302739.png)

1. 命令格式：${作用域对象别名.共享数据}
2. 命令作用：
   - EL表达式是EL工具包提供一种特殊命令格式【表达式命令格式】
   - EL表达式在JSP文件上使用
   - 负责在JSP文件上从作用域对象读取指定的共享数据并输出到响应体

###  四、EL表达式 --- 作用域对象别名

1. JSP文件可以使用的作用域对象

   - ServletContext              application:  全局作用域对象

   - HttpSession                  session:       会话作用域对象

   - HttpServletRequest       request:      请求作用域对象

   - PageContext         pageContext: 当前页作用域对象，这是JSP文件独有的作用域对象；Servlet中不存在。在当前页作用域对象存放的共享数据仅能在当前JSP文件中使用，不能共享给其他Servlet或则其他JSP文件。

     真实开发过程，主要用于JSTL标签与JSP文件之间数据共享数据。

     JSTL------->pageContext---->JSP

2. EL表达式提供作用域对象别名

   |     JSP     |            EL表达式            |
   | :---------: | :----------------------------: |
   | application | ${applicationScope.共享数据名} |
   |   session   |   ${sessionScope.共享数据名}   |
   |   request   |   ${requestScope.共享数据名}   |
   | pageContext |    ${pageScope.共享数据名}     |

### 五、EL表达式将引用对象属性写入到响应体

1. 命令格式：${作用域对象别名.共享数据名.属性名}
2. 命令作用：从作用域对象读取指定共享数据关联的引用对象的属性值；并自动将属性的结果写入到响应体
3. 属性名：一定要去引用类型属性名完全一致（区分大小写）
4. EL表达式没有提供遍历集合方法，因此无法从作用域对象读取集合内容输出

```java
public class Student {
    private Integer sid;
    private String sname;
    public Student() {
    }
    public Student(Integer sid, String sanem) {
        this.sid = sid;
        this.sname = sanem;
    }
    public Integer getSid() {
        return sid;
    }
    public void setSid(Integer sid) {
        this.sid = sid;
    }
    public String getSname() {
        return sname;
    }
    public void setSname(String sname) {
        this.sname = sname;
    }
}
```

```java
public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.创建一个引用类型实例对象
        Student stu = new Student(20, "allen");
        //2.将引用类型对象存入到请求作用域对象作为共享数据
        request.setAttribute("key", stu);
        //3.请求转发，向Tomcat申请调用index_1.jsp
        request.getRequestDispatcher("/index_1.jsp").forward(request, response);
    }
}
```

```jsp
<%@ page import="com.example.entity.Student" %><%--
  Created by IntelliJ IDEA.
  User: Amadeus
  Date: 2022/1/8
  Time: 17:32
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--传统写法-->
<%
  Student stu = (Student) request.getAttribute("key");
%>
学员编号:<%=stu.getSid()%><br>
学员姓名:<%=stu.getSname()%>

<hr>
<!--EL表达式-->
学员编号:${requestScope.key.sid}<br>
学员姓名:${requestScope.key.sname}
```

![image-20220108174214310](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108174214310.png)

### 六、EL表达式简化版

1. 命令格式： ${共享数据名} 

2. 命令作用： EL表达式允许开发人员开发时省略作用域对象别名 

3. 工作原理：EL表达式简化版由于没有指定作用域对象，所以在执行时采用【猜】算法

   - 首先到【pageContext】定位共享数据，如果存在直接读取输出并结束执行

   - 如果在【pageContext】没有定位成功，到【request】定位共享数据，如果存在直接读取输出并结束执行

   - 如果在【request】没有定位成功，到【session】定位共享数据，如果存在直接读取输出并结束执行

   - 如果在【session】没有定位成功，到【application】定位共享数据，如果存在直接读取输出并结束执行

   - 如果在【application】没有定位成功，返回null

     当前页作用域对象--->请求--->会话--->全局

4.  存在隐患：

   - 容易降低程序执行速度【南辕北辙】
   - 容易导致数据定位错误

5. 应用场景：设计目的，就是简化从pageContext读取共享数据并输出难度

6. EL表达式简化版尽管存在很多隐患，但是在实际开发过程中，开发人员为了节省时间，一般都使用简化版，拒绝使用标准版

```java
public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.向当前用户session添加一个共享数据
        HttpSession session = request.getSession();
        session.setAttribute("key", "allen");

        //2.请求转发，申请调用index_1.jsp
        request.getRequestDispatcher("/index_1.jsp").forward(request, response);
    }
}
```

```jsp
<!--index_1.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
标准版EL表达式输出session中key值:${sessionScope.key}<br>
简化版EL表达式输出session中key值:${key}
```

![image-20220108175921163](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108175921163.png)

```java
public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.向当前用户session添加一个共享数据
        HttpSession session = request.getSession();
        session.setAttribute("key", "allen");

        //2.向当前请求作用域对象添加一个共享数据
        request.setAttribute("key", "mike");

        //3.请求转发，申请调用index_1.jsp
        request.getRequestDispatcher("/index_1.jsp").forward(request, response);
    }
}
```

```jsp
<!--index_1.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
标准版EL表达式输出session中key值:${sessionScope.key}<br>
简化版EL表达式输出session中key值:${key}
```

![image-20220108180118317](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108180118317.png)

### 七、EL表达式-----支持运算表达式

1. 前提：在JSP文件有时需要将读取共享数据进行一番运算之后，将运算结果写入到响应体

2. 运算表达式:

   - 数学运算

   - 关系运算

     |  >   |  >=  |  ==  |  <   |  <=  |  !=  |
     | :--: | :--: | :--: | :--: | :--: | :--: |
     |  gt  |  ge  |  eq  |  lt  |  le  |  !=  |

   - 逻辑运算：&&    ||    !

```java
public class OneServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("key1", "100");
        request.setAttribute("key2", 200);
        request.getRequestDispatcher("index_1.jsp").forward(request, response);
    }
}
```

```jsp
<!--index_1.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--将作用域对象中共享数据读取出来相加，将相加结果写入到响应体-->
<%
    String num1 = (String) request.getAttribute("key1");
    Integer num2 = (Integer) request.getAttribute("key2");
    int sum = Integer.valueOf(num1) + num2;
%>
传统的Java命令计算后的结果：<%=sum%><br>
EL表达式计算后的结果：${key1+key2}
```

![image-20220108182833103](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108182833103.png)

```java
public class TwoServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("age", "25");
        request.getRequestDispatcher("index_2.jsp").forward(request, response);
    }
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--传统Java命令方式实现关系运算输出-->
<%
  String age = (String) request.getAttribute("age");
  if(Integer.valueOf(age) >= 18){
%>
      欢迎光临<br>
<%
  } else {
%>
      谢绝入内<br>
<%
  }
%>

<hr>
EL表达式方式实现关系运算输出:${age >= 18 ? "欢迎光临" : "谢绝入内"}<br>
EL表达式方式实现关系运算输出:${age ge 18 ? "欢迎光临" : "谢绝入内"}
```

![image-20220108183937535](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108183937535.png)

### 八、EL表达式提供内置对象

##### 1 param

1. 命令格式: ${param.请求参数名}

2. 命令作用：

   - 通过请求对象读取当前请求包中请求参数内容
   - 并将请求参数内容写入到响应体

3. 代替命令

   index_1.jsp

   发送请求：Http://localhost:8080/myWeb/index_1.jsp?userName=mike&password=123

   ```jsp
   <%
   	String userName =   request.getParameter("userName");
   	String password =   request.getParameter("password");
   %>
   <%=userName%>
   <%=password%>
   ```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--
    Http://localhost:8080/myWeb/index_1.jsp?userName=mike&password=123
-->
来访者的姓名:${param.userName}<br>
来访者的密码:${param.password}
```

![image-20220108185155571](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108185155571.png)

##### 2 paramValues

1. 命令格式：${paramValues.请求参数名[下标]}

2. 命令作用:

   - 如果浏览器发送的请求参数是[一个请求参数关联多个值]，此时可以通过paramVaues读取请求参数下指定位置的值，并写入到响应体

3. 代替命令：

   http://localhost:8080/myWeb/index_2.jsp?pageNo=1&pageNo=2&pageNo=3

   此时pageNo请求参数在请求包以数组形式存在

   ```jsp
   pageNo:[1,2,3]
   <%
   	String array[]= request.getParameterValues("pageNo");
   %>
   第一个值:<%=array[0]%>
   第二个值:<%=array[1]%>
   ```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--
http://localhost:8080/myWeb/index_2.jsp?deptNo=10&deptNo=20&deptNo=30
-->
第一个部门编号:${paramValues.deptNo[0]}<br>
第二个部门编号:${paramValues.deptNo[1]}<br>
第三个部门编号:${paramValues.deptNo[2]}<br>
```

![image-20220108190025048](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108190025048.png)

### 九、应用

在线考试管理系统---通过questionId显示试题详细信息EL版

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
  <title>EL_Question_Update</title>
</head>
<body>
<center>
  <form action="/myWeb/question/update" method="get">
    <table border="2">
      <tr>
        <td>题目编号:</td>
        <td><input type="text" name="questionId" value="${requestScope.key.questionId}" readOnly></td>
      </tr>
      <tr>
        <td>题目:</td>
        <td><input type="text" name="title" value="${requestScope.key.title}"></td>
      </tr>
      <tr>
        <td>A:</td>
        <td><input type="text" name="optionA" value="${requestScope.key.optionA}"></td>
      </tr>
      <tr>
        <td>B:</td>
        <td><input type="text" name="optionB" value="${requestScope.key.optionB}"></td>
      </tr>
      <tr>
        <td>C:</td>
        <td><input type="text" name="optionC" value="${requestScope.key.optionC}"></td>
      </tr>
      <tr>
        <td>D:</td>
        <td><input type="text" name="optionD" value="${requestScope.key.optionD}"></td>
      </tr>
      <tr>
        <td>正确答案:</td>
        <td>
          <input type="radio" name="answer" value="A" ${"A" eq requestScope.key.answer ? "checked" : ""} >A
          <input type="radio" name="answer" value="B" ${"B" eq requestScope.key.answer ? "checked" : ""} >B
          <input type="radio" name="answer" value="C" ${"C" eq requestScope.key.answer ? "checked" : ""} >C
          <input type="radio" name="answer" value="D" ${"D" eq requestScope.key.answer ? "checked" : ""} >D
        </td>
      </tr>
      <tr>
        <td><input type="submit" value="更新试题"/></td>
        <td><input type="reset" ></td>
      </tr>
    </table>
  </form>
</center>
</body>
</html>
```

### 十、EL表达式常见异常

```java
javax.el.PropertyNotFoundException;//在对象中没有找到指定属性
```

### 十一、在线考试管理系统

```
开发任务：随机出题
任务描述：用户点击【参加考试】，系统【随机】提取四道考试题，交给用户
开发任务: 在线阅卷
        1.根据用户提供答案与正确答案比较得到用户分数
        2.将用户分数交给scor.jsp做输出
```

##### 1 自动出题

```mysql
#查询职员信息，按照部门编号排序，截取前两个
select * from emp order by deptno desc limit 0,2;
#查询职员信息，按照职员编号倒叙，截取前两个
select * from emp order by empno asc limit 0,2;
#查询职员信息，按照工资倒叙，截取前两个
select * from emp order by sal asc limit 0,2;

#如果每次查询时，排序字段不同，然后在截取得到数据行内容往往不一样
select rang();#随机返回0---1之间小数
#rand() 0.5--->5  order by 5
# 0.9--->order by 1 
select * from question order by rand() limit 0,4;
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <ul>
    <li>用户信息管理
      <ol>
        <li><a href="/myWeb/user_Add.html" target="right">用户信息注册</a> </li>
        <li><a href="/myWeb/user/find" target="right">用户信息查询</a></li>
      </ol>
    </li>
    <li>试题信息管理
      <ol>
        <li><a href="/myWeb/question_Add.html" target="right">试题注册</a> </li>
        <li><a href="/myWeb/question/find" target="right">试题查询</a></li>
      </ol>
    </li>
    <li>考试管理
      <ol>
        <li><a href="/myWeb/question/rand" target="right">参加考试</a> </li>
        <li><a href="/myWeb/question/find" target="right"></a></li>
      </ol>
    </li>
  </ul>
</body>
</html>
```

```java
public class QuestionRandServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.调用Dao对象随机从question表拿出4道题目
        List<Question> list = QuestionDao.findRand();
        //2.将4道题目添加到request作为共享数据
        request.setAttribute("key", list);
        //3.请求转发，申请exam.jsp将4道题目写入到响应体
        request.getRequestDispatcher("/exam.jsp").forward(request, response);
    }
}
```

```java
public class QuestionDao {
    public static List<Question> findRand(){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        List<Question> questionList = new ArrayList<>();
        try {
            String sql = "select * from question order by rand() limit 0,4";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            rs = ps.executeQuery();
            while(rs.next()){
                Integer questionId = rs.getInt("questionId");
                String title = rs.getString("title");
                String optionA = rs.getString("optionA");
                String optionB = rs.getString("optionB");
                String optionC = rs.getString("optionC");
                String optionD = rs.getString("optionD");
                String answer = rs.getString("answer");
                Question question = new Question(questionId, title, optionA, optionB, optionC, optionD, answer);
                questionList.add(question);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return questionList;
    }
}
```

```jsp
<!--exam.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <center>
        <form action="/myWeb/exam">
            <table border="2">
                <tr>
                    <td>试题编号</td>
                    <td>题目信息</td>
                    <td>A</td>
                    <td>B</td>
                    <td>C</td>
                    <td>D</td>
                    <td>答案</td>
                </tr>
                <%
                   List<Question> questionList = (List<Question>) request.getAttribute("key");
                   for(Question question : questionList){
                %>
                    <tr>
                        <td><%=question.getQuestionId()%></td>
                        <td><%=question.getTitle()%></td>
                        <td><%=question.getOptionA()%></td>
                        <td><%=question.getOptionB()%></td>
                        <td><%=question.getOptionC()%></td>
                        <td><%=question.getOptionD()%></td>
                        <td>
                            <input type="radio" name="answer_<%=question.getQuestionId()%>" value="A">A
                            <input type="radio" name="answer_<%=question.getQuestionId()%>" value="B">B
                            <input type="radio" name="answer_<%=question.getQuestionId()%>" value="C">C
                            <input type="radio" name="answer_<%=question.getQuestionId()%>" value="D">D
                        </td>
                    </tr>
                <%
                    }
                %>
                <tr align="center">
                    <td colspan="3"><input type="submit" value="交卷"></td>
                    <td colspan="4"><input type="reset" value="重做"></td>
                </tr>
            </table>
        </form>
    </center>
</body>
</html>
```

![image-20220108201127994](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108201127994.png)

##### 2 在线阅卷

```java
public class ExamServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //分数
        int score = 0;
        //1.从当前用户私人储物柜得到系统提供四道题目信息
        HttpSession session = request.getSession(false);
        List<Question> questionList = (List<Question>) session.getAttribute("key");
        //2.从请求包读取用户对于四道题目给出答案
        for (Question question : questionList){
            String answer = question.getAnswer();
            Integer questionId = question.getQuestionId();
            String userAnswer = (String) request.getParameter("answer_" + questionId);
            //3.判分
            if(userAnswer.equals(answer)){
                score += 25;
            }
        }
        //4.将分数写入到request中，作为共享数据
        request.setAttribute("info", "本次考试成绩: " + score);
        //5.请求转发调用jsp将用户本次考试分数写入到响应体
        request.getRequestDispatcher("info.jsp").forward(request, response);
    }
}
```

![image-20220108205827417](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108205827417.png)

![image-20220108205834065](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108205834065.png)

# MVC开发规则

### 一、介绍

1. MVC开发规则制定了互联网通信开发过程中必须出现的角色有哪些
2. MVC开发规则制定了互联网通信开发过程中必须出现的角色所要担负的职责
3. MVC开发规则制定了互联网通信开发过程中必须出现角色的出场顺序

### 二、角色

1. DAO对象：DAO对象提供某张表文件的操作细节，降低对表文件操作难度；避免反复开发表文件操作的代码，提高代码复用性
2. Service对象：服务对象，提供【业务】的具体解决方案；service对象一个方法指定一个业务的解决方案；避免业务开发重复性开发行为，提供复用性；网站每一个业务都有一个独立标准解决方案

### 三、业务

浏览器向Http服务器发送请求

用户向网站发送请求

举个例子：张三用户发送请求：要求在服务端实现将张三账户3000元钱转给李四账户

业务处理方案:

1. 判断"张三"是否是当前系统中用户；
2. 判断"李四"是否是当前系统中用户；
3. 读取"张三账户余额"，判断余额是否充足；
4. 读取"李四账户余额"，背账；
5. 更新"张三账户余额  -3000"；
6. 更新 "李四账户余额  +3000 。

### 四、业务特征

1. 真实业务场景中，一个业务往往包含多个分支任务；因此解决业务开发工作量往往比较巨大
2. 真实业务场景中，只有所有分支任务都能顺利成功解决，才可以认为当前业务处理成功

### 五、解决业务开发困恼

1. 一个业务可能在网站的多个地方重复出现，如果不做【封装】，增加开发难度，进行业务解决代码重复性开发
2. 【百人有百味】，不同程序员面对同一个业务时，给出解决方案往往有偏差，导致最终解决数据会有偏差

### 六、MVC开发规则----互联网通信开发过程中必须出现角色有哪些

一次互联网开发过程，必须出现角色有三个：

1. C	contorller object； 控制层对象   （servlet对象）
2. M    model      object； 业务模型对象 （Service对象）
3. V,    view         object； 视图层对象    (jsp  or  HttpServletResponse)

### 七、MVC开发规则------互联网通信开发过程中必须出现角色担负职责

1. C（servlet对象）：
   - 【可以】调用【请求对象】读取【请求包】参数信息
   - 【必须】调用【Service对象】处理业务
   - 【必须】调用【 视图层对象】将结果写入到响应体
2. M（service对象）：
   - 处理业务中所有分支任务
   - 根据分支任务执行情况判断业务是否处理成功
   - 必须通过return将处理结果返回给【控制层对象】
3. V（jsp/HttpServletResponse）：
   - 【禁止参与业务处理】
   - 唯一任务将处理结果写入到响应体

### 八、互联网通信开发过程中必须出现角色的出场顺序

请求调用顺序：

浏览器---发送请求--->Servlet------>Service------>DeptDao/EmpDao

响应顺序：

DeptDao/EmpDao---分支任务结果--->Service------>Servlet------>View------>响应体---(Tomcat负责推送)--->浏览器

![image-20220108214332630](04JSP_EL_MVC_AJAX_JSON.assets\image-20220108214332630.png)

# AJAX

### 一、全局刷新和局部刷新

全局刷新：整个浏览器被新的数据覆盖；在网络中传输大量的数据；浏览器需要加载，渲染页面。

全局刷新原理：

1. 必须由浏览器亲自向服务端发送请求协议包
2. 这个行为导致服务端直接将【响应包】发送到浏览器内存中
3. 这个行为导致浏览器内存中原有内容被覆盖掉
4. 这个行为导致浏览器在展示数据时候，只有响应数据可以展示



局部刷新：在浏览器器的内部，发起请求，获取数据，改变页面中的部分内容；其余的页面无需加载和渲染。 网络中数据传输量少， 给用户的感受好。

局部刷新原理：

1. 不能由浏览器发送请求给服务端
2. 浏览器委托浏览器内存中一个脚本对象代替浏览器发送请求
3. 这个行为导致导致服务端直接将【响应包】发送脚本对象内存中
4. 这个行为导致脚本对象内容被覆盖掉，但是此时浏览器内存中绝大部分内容没有受到任何影响
5. 这个行为导致浏览器在展示数据时候,同时展示原有数据和响应数据

ajax是用来做局部刷新的。局部刷新使用的核心对象是 异步对象（XMLHttpRequest）
这个异步对象是存在浏览器内存中的 ，使用javascript语法创建和使用XMLHttpRequest对象。

### 二、AJAX

##### 1 什么是AJAX

- AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。 
- AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分页面内容的新方法 
- AJAX 不是新的编程语言，而是使用现有技术混合使用的一种新方法。
- AJAX 中使用的技术有 JavaScript、html、dom、xml、css 等。主要是 JavaScript、XML
- JavaScript：使用脚本对象 XMLHttpRequest 发送请求，接收响应数据 
- XML：发送和接收的数据格式，现在使用 JSON 
- AJAX 不单需要前端的技术，同时需要后端（服务器）的配合。服务器需要提供数据，数据是 AJAX 请求的响应结果。

##### 2 AJAX 异步实现步骤

1. 创建异步对象方式

   ```java
   var xmlHttp = new XMLHttpRequest()；
   ```

2. 给异步对象绑定事件

   - onreadystatechange：当异步对象发起请求，获取了数据都会触发这个事件。这个事件需要指定一个函数， 在函数中处理状态的变化。

     ```javascript
     btn.onclick = fun1()
         function fun1(){
          alert("按钮单击");
         }
     
         //例如：
         xmlHttp.onreadystatechange= function(){
               //处理请求的状态变化。
     		 if(xmlHttp.readyState == 4 && xmlHttp.status== 200 ){
                    //可以处理服务器端的数据，更新当前页面
     			  var data = xmlHttp.responseText;
     			  document.getElementById("name").value= data;
     		 }
         }
     ```

   -  异步对象的属性 readyState 表示异步对象请求的状态变化

     - 0：创建异步对象时； new XMLHttpRequest();
     - 1：初始化异步请求对象； xmlHttp.open(请求方式，请求地址，true)
     - 2：异步对象发送请求；xmlHttp.send()
     - 3：异步对象接收应答数据；从服务端返回数据。XMLHttpRequest 内部处理。
     - 4：异步请求对象已经将数据解析完毕；此时才可以读取数据。（在4的时候，开发人员做什么？更新当前页面。）

   - status属性：表示网络请求的状况的

     - 200："OK" | "通信成功"
     - 404：未找到页面
     - 500：服务器端代码出错

3. 初始化请求参数

   - 异步的方法open()
   - open(method, url, async) ：初始化异步请求对象
   - method：请求的类型；GET 或 POST
   - url：服务器端的访问地址（例如servlet地址）
   - async：true（异步）或 false（同步）

4. 发送请求 

   ```javascript
   xmlHttp.send();
   ```

5. 获取服务器端返回的数据 

   - 如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。
   - responseText：获得字符串形式的响应数据
   - responseXML：获得 XML 形式的响应数据
   - 回调：当请求的状态变化时，异步对象会自动调用onreadystatechange事件对应的函数

### 三、AJAX实例一

##### 1 全局刷新计算BMI

- 需求：计算某个用户的 BMI，用户在JSP输入自己的身高，体重；servlet中计算 BMI，并显示 BMI 的计算结果和建议。
- BMI 指数（即身体质量指数，英文为 BodyMassIndex，简称 BMI），是用 体重公斤数 / 身高米数平方 得出的数字，是目前国际上常用的衡量人体胖瘦程度以及是否健康的一个标准
- 成人的 BMI 数值： 
  - 过轻：低于18.5
  - 正常：18.5 - 23.9 
  - 过重：24 - 27 
  - 肥胖：28 - 32
  - 非常肥胖：高于32

```jsp
<!--index.jsp-->
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <title>全局刷新</title>
</head>
<body>
    <p>全局刷新计算BMI</p>
    <form action="/myWeb/bmi" method="get">
        姓名:<input type="text" name="name"><br>
        体重(公斤):<input type="text" name="weight"><br>
        身高(米):<input type="text" name="height"><br>
        <input type="submit" value="提交">
        <input type="reset" value="重置">
    </form>
</body>
</html>
```

```java
//servlet
public class BMIServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.接收请求参数
        String name = (String) request.getParameter("name");
        String weight = (String) request.getParameter("weight");
        String height = (String) request.getParameter("height");
        //2.计算BMI: bmi = weight / (height * height);
        float w = Float.valueOf(weight);
        float h = Float.valueOf(height);
        float bmi = w / (h * h);
        //3.判断BMI的范围
        String msg = "";
        if(bmi <= 18.5){
            msg = "您比较瘦";
        } else if(bmi <= 23.9){
            msg = "您的BMI是正常的";
        } else if(bmi <= 27){
            msg = "您的身体过重";
        } else if (bmi <= 32){
            msg = "您的身体肥胖";
        } else {
            msg = "您的身体过于肥胖";
        }
        System.out.println("msg = " + msg);
        msg = "您好" + name + "先生/女士，您的BMI是：" + bmi + "，" + msg;
        System.out.println(msg);
        //4.把数据存入到request
        request.setAttribute("msg", msg);
        //5.转发到新的页面
        request.getRequestDispatcher("/result.jsp").forward(request, response);
    }
}
```

```jsp
<!--result.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>结果页面</title>
</head>
<body>
  <p>显示bmi计算结果</p>
  <h3>${requestScope.msg}</h3>
</body>
</html>
```

```xml
<!--web.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>BMIServlet</servlet-name>
        <servlet-class>com.example.BMIServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>BMIServlet</servlet-name>
        <url-pattern>/bmi</url-pattern>
    </servlet-mapping>
</web-app>
```

![image-20220109152523220](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109152523220.png)

![image-20220109152526689](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109152526689.png)

##### 2 通过HttpServletResponse输出数据

```jsp
<!--bmiPrint.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html>
<head>
  <title>通过HttpServletResponse输出数据</title>
</head>
<body>
<p>通过HttpServletResponse输出数据</p>
<form action="/myWeb/bmiprint" method="get">
  姓名:<input type="text" name="name"><br>
  体重(公斤):<input type="text" name="weight"><br>
  身高(米):<input type="text" name="height"><br>
  <input type="submit" value="提交">
  <input type="reset" value="重置">
</form>
</body>
</html>
```

```java
//通过HttpServletResponse输出数据
public class BMIPrintServlet extends HttpServlet {// /bmiprint
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //1.接收请求参数
        String name = (String) request.getParameter("name");
        String weight = (String) request.getParameter("weight");
        String height = (String) request.getParameter("height");
        //2.计算BMI: bmi = weight / (height * height);
        float w = Float.valueOf(weight);
        float h = Float.valueOf(height);
        float bmi = w / (h * h);
        //3.判断BMI的范围
        String msg = "";
        if(bmi <= 18.5){
            msg = "您比较瘦";
        } else if(bmi <= 23.9){
            msg = "您的BMI是正常的";
        } else if(bmi <= 27){
            msg = "您的身体过重";
        } else if (bmi <= 32){
            msg = "您的身体肥胖";
        } else {
            msg = "您的身体过于肥胖";
        }
        System.out.println("msg = " + msg);
        msg = "您好" + name + "先生/女士，您的BMI是：" + bmi + "，" + msg;
        System.out.println(msg);
        //4.使用HttpServletResponse输出数据
        response.setContentType("text/html;charset=utf-8");
        PrintWriter writer = response.getWriter();//获取PrintWriter
        writer.println(msg);//输出数据
        writer.flush();//清空缓存
        writer.close();//关闭close
    }
}
```

![image-20220109154140955](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109154140955.png)

![image-20220109154144934](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109154144934.png)

##### 3 使用AJAX请求，计算BMI

```
使用AJAX的局部刷新
1.新建JSP，使用XMLHttpResquest异步对象
    使用异步对象有四个步骤：
        1.创建
        2.绑定事件
        3.初始请求
        4.发送请求
2.创建服务器的Servlet，接收并处理数据，把数据输出给异步对象。
```

```jsp
<!--index.jsp-->
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
  <title>局部刷新-AJAX</title>
  <script type="text/javascript">
    //使用内存中的异步对象，代替浏览器发起请求。异步对象使用js创建和管理的
    function doAjax(){
      //1.创建异步对象
      var xmlHttp = new XMLHttpRequest();
      //2.绑定事件
      xmlHttp.onreadystatechange = function (){
        //处理服务端返回的数据，更新当前页面
        // alert("readyState属性值=====" + xmlHttp.readyState + "| status = " + xmlHttp.status)
        if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
          // alert(xmlHttp.responseText)
          var data = xmlHttp.responseText;
          document.getElementById("myData").innerText = data;
        }
      }
      //3.初始请求数据
      //获取dom对象的value属性值
      var name = document.getElementById("name").value;
      var weight = document.getElementById("weight").value;
      var height = document.getElementById("height").value;
      //bmiPrint?name=李四&weight=82&height=1.8
      var param = "name=" + name + "&weight=" + weight + "&height=" + height;
      // alert(param);
      xmlHttp.open("get", "bmiAjax?"+param, true);
      //4.发起请求
      xmlHttp.send();
    }
  </script>
</head>
<body>
  <p>局部刷新AJAX-计算bmi</p>
  <div>
    <!--没有使用form表单-->
    姓名:<input type="text" id="name"><br>
    体重(公斤):<input type="text" id="weight"><br>
    身高(米):<input type="text" id="height"><br>
    <input type="button" value="计算BMI" onclick="doAjax()">
    <br><br>
    <div id="myData">等待加载数据.....</div>
  </div>
</body>
</html>
```

```java
public class BMIAJAXServlet extends HttpServlet {// /bmiAjax
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.接收请求参数
        String name = (String) request.getParameter("name");
        String weight = (String) request.getParameter("weight");
        String height = (String) request.getParameter("height");
        //2.计算BMI: bmi = weight / (height * height);
        float w = Float.valueOf(weight);
        float h = Float.valueOf(height);
        float bmi = w / (h * h);
        //3.判断BMI的范围
        String msg = "";
        if(bmi <= 18.5){
            msg = "您比较瘦";
        } else if(bmi <= 23.9){
            msg = "您的BMI是正常的";
        } else if(bmi <= 27){
            msg = "您的身体过重";
        } else if (bmi <= 32){
            msg = "您的身体肥胖";
        } else {
            msg = "您的身体过于肥胖";
        }
        System.out.println("msg = " + msg);
        msg = "您好" + name + "先生/女士，您的BMI是：" + bmi + "，" + msg;
        System.out.println(msg);
        //4.响应ajax需要的数据，使用HttpServletResponse输出数据
        response.setContentType("text/html;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.println(msg);
        pw.flush();
        pw.close();
    }
}
```

![image-20220109163756700](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109163756700.png)

![image-20220109163800769](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109163800769.png)

### 四、AJAX实例二

- 需求：用户在文本框架输入省份的编号 id，在其他文本框显示省份名称

- 项目环境准备：

  - 数据库：springdb

  - 数据表：

    ```mysql
    #省份信息表：
    CREATE TABLE `province` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `name` varchar(255) DEFAULT NULL COMMENT '省份名称',
     `jiancheng` varchar(255) DEFAULT NULL COMMENT '简称',
     `shenghui` varchar(255) DEFAULT NULL,
     PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
    
    #城市信息表：
    CREATE TABLE `city` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `name` varchar(255) DEFAULT NULL,
     `provinceid` int(11) DEFAULT NULL,
     PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8;
    ```

![image-20220109173434164](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109173434164.png)

```jsp
<!--index.jsp-->
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <title>ajax根据省份Id获取名称</title>
    <script type="text/javascript">
        function search(){
            //发起AJAX请求，传递参数给服务器，服务器返回数据
            //1.创建异步对象
            var xmlHttp = new XMLHttpRequest();
            //2.绑定事件
            xmlHttp.onreadystatechange = function (){
                if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                    document.getElementById("proname").value = xmlHttp.responseText;
                }
            }
            //3.初始异步对象
            //获取proid文本框的值
            var proid = document.getElementById("proid").value;
            xmlHttp.open("get", "queryProvice?proid="+proid, true);
            //4.发送请求
            xmlHttp.send();
        }
    </script>
</head>
<body>
    <p align="center">ajax根据省份Id获取名称</p>
    <table align="center">
        <tr>
            <td>省份编号：</td>
            <td>
                <input type="text" id="proid">
                <input type="button" value="搜索" onclick="search()">
            </td>
        </tr>
        <tr>
            <td>省份名称：</td>
            <td><input type="text" id="proname"></td>
        </tr>
    </table>
</body>
</html>
```

```java
public class QueryProvinceServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("响应了AJAX请求");
        String name = "";
        //处理get请求
        String proidStr = request.getParameter("proid");
        System.out.println("proid==="+proidStr);
        if(proidStr != null && !"".equals(proidStr.trim())){
            //访问Dao，查询数据库
            name = ProvinceDao.queryProvinceNameById(Integer.valueOf(proidStr));
        }
        //使用HttpServletResponse输出数据
        //响应ajax需要的数据，使用HttpServletResponse输出数据
        response.setContentType("text/html;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.println(name);
        pw.flush();
        pw.close();
    }
}
```

```java
//使用JDBC访问数据库
public class ProvinceDao {
    //根据Id获取名称
    public static String queryProvinceNameById(Integer provinceId){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        String name = "";
        try {
            conn = JdbcUtil.getConnection();
            String sql = "select name from province where id=?";
            ps = conn.prepareStatement(sql);
            ps.setInt(1, provinceId);
            rs = ps.executeQuery();
            if(rs.next()){
                name = rs.getString("name");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return name;
    }
}
```

![image-20220109173713129](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109173713129.png)

![image-20220109173716648](04JSP_EL_MVC_AJAX_JSON.assets\image-20220109173716648.png)

### 五、JSON

- ajax发起请求---- Servlet（返回的一个json格式的字符串 { name:"河北", jiancheng:"冀","shenghui":"石家庄"}）
- JSON分类：
  - json对象，JSONObject，这种对象的格式："名称:值"，也可以看做是 "key:value" 格式。
  - json数组，JSONArray，基本格式  [{ name:"河北", jiancheng:"冀","shenghui":"石家庄"} , { name:"山西", jiancheng:"晋","shenghui":"太原"} ]
- 为什么使用JSON（优点）：
  - json格式好理解
  - json格式数据在多种语言中，比较容易处理；使用java，javascript读写json格式的数据比较容易。
  - json格式数占用的空间小，在网络中传输快，用户的体验好。
- 处理json的工具库：
  - gson（google）
  - fastjson（阿里）：速度快，不是符合JSON处理规范的
  - jackson：性能好，规范好
  - json-lib：性能差，依赖多
- 在js中，可以把json格式的字符串，转为json对象， json中的key，就是json对象的属性名。

1. jackson使用

   ```java
   public class TestJson {
       public static void main(String[] args) throws JsonProcessingException {
           //使用 jackson 把 java对象转为json格式
           Province province = new Province();
           province.setId(1);
           province.setName("江苏");
           province.setJiancheng("苏");
           province.setShenghui("南京");
   
           //使用jackson 把 province对象 转为 json
           ObjectMapper om = new ObjectMapper();
           //writeValueAsString:把参数的java对象转为json格式的字符串
           String json = om.writeValueAsString(province);
           System.out.println("json===" + json);
           //json==={"id":1,"name":"江苏","jiancheng":"苏","shenghui":"南京"}
       }
   }
   ```

2. ajax请求使用json格式的数据

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>使用JSON格式的数据</title>
    <script type="text/javascript">
        function doSearch(){
            //1.创建异步对象
            var xmlHttp = new XMLHttpRequest();
            //2.绑定事件
            xmlHttp.onreadystatechange = function (){
                if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                    var data = xmlHttp.responseText;
                    //eval是执行括号中的代码，把json字符串转为json对象
                    var jsonObj = eval("(" + data + ")")
                    //更新Dom对象
                    document.getElementById("proname").value = jsonObj.name;
                    document.getElementById("projc").value = jsonObj.jiancheng;
                    document.getElementById("prosh").value = jsonObj.shenghui;
                }
            }
            //3.初始异步对象
            //获取proid文本框的值
            var proid = document.getElementById("proid").value;
            xmlHttp.open("get", "queryJson?proid="+proid, true);
            //4.发送请求
            xmlHttp.send();
        }
    </script>
</head>
<body>
    <p>ajax请求使用json格式的数据</p>
    <table border="2">
        <tr>
            <td>省份编号</td>
            <td>
                <input type="text" id="proid">
                <input type="button" value="搜索" onclick="doSearch()">
            </td>
        </tr>
        <tr>
            <td>省份名称</td>
            <td><input type="text" id="proname"></td>
        </tr>
        <tr>
            <td>省份简称</td>
            <td><input type="text" id="projc"></td>
        </tr>
        <tr>
            <td>省会名称</td>
            <td><input type="text" id="prosh"></td>
        </tr>
    </table>
</body>
</html>
```

```java
public class QueryJsonServlet extends HttpServlet { // queryJson
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //默认值,{}:表示json格式的数据
        String json = "{}";
        //获取请求参数，省会的Id
        String strProid = request.getParameter("proid");
        //判断proid有值时，调用dao查询参数
        if(strProid != null && strProid.trim().length() > 0){
            Province province = ProvinceDao.queryProvinceById(Integer.valueOf(strProid));
            //需要使用jackson 把 province对象 转为 json
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(province);
        }
        //把获取的数据，通过网络传给ajax中的异步对象，响应结果数据
        //指定服务器端(servlet)返回给浏览器的是JSON格式的数据
        response.setContentType("application/json;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.println(json);//输出数据，会交付给ajax中responseText属性
        pw.flush();
        pw.close();
    }
}

```

```java
//使用JDBC访问数据库
public class ProvinceDao {
    //根据Id获取完整的Province对象
    public static Province queryProvinceById(Integer provinceId){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        Province province = null;
        try {
            conn = JdbcUtil.getConnection();
            String sql = "select * from province where id=?";
            ps = conn.prepareStatement(sql);
            ps.setInt(1, provinceId);
            rs = ps.executeQuery();
            while (rs.next()){
                province = new Province();
                province.setId(rs.getInt("id"));
                province.setName(rs.getString("name"));
                province.setJiancheng(rs.getString("jiancheng"));
                province.setShenghui(rs.getString("shenghui"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return province;
    }
}
```

![image-20220110151541480](04JSP_EL_MVC_AJAX_JSON.assets\image-20220110151541480.png)

![image-20220110151545244](04JSP_EL_MVC_AJAX_JSON.assets\image-20220110151545244.png)

```jsp
<!--通过回调函数处理返回的数据-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>使用JSON格式的数据</title>
    <script type="text/javascript">
        function doSearch(){
            //1.创建异步对象
            var xmlHttp = new XMLHttpRequest();
            //2.绑定事件
            xmlHttp.onreadystatechange = function (){
                if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
                    var data = xmlHttp.responseText;
                    //eval是执行括号中的代码，把json字符串转为json对象
                    var jsonObj = eval("(" + data + ")")
                    callback(jsonObj);
                }
            }
            //3.初始异步对象
            //获取proid文本框的值
            var proid = document.getElementById("proid").value;
            xmlHttp.open("get", "queryJson?proid="+proid, true);
            //4.发送请求
            xmlHttp.send();
        }
        
        //定义函数，处理服务器端返回的数据
        function callback(json){
            //更新Dom对象
            document.getElementById("proname").value = json.name;
            document.getElementById("projc").value = json.jiancheng;
            document.getElementById("prosh").value = json.shenghui;
        }
    </script>
</head>
<body>
    <p>ajax请求使用json格式的数据</p>
    <table border="2">
        <tr>
            <td>省份编号</td>
            <td>
                <input type="text" id="proid">
                <input type="button" value="搜索" onclick="doSearch()">
            </td>
        </tr>
        <tr>
            <td>省份名称</td>
            <td><input type="text" id="proname"></td>
        </tr>
        <tr>
            <td>省份简称</td>
            <td><input type="text" id="projc"></td>
        </tr>
        <tr>
            <td>省会名称</td>
            <td><input type="text" id="prosh"></td>
        </tr>
    </table>
</body>
</html>
```

### 六、同步和异步

- open(method, url, async) ：初始化异步请求对象
- method：请求的类型；GET 或 POST
- url：服务器端的访问地址（例如servlet地址）
- async：true（异步）或 false（同步）
- true：异步处理请求；使用异步对象发起请求后，不用等待数据处理完毕，就可以执行其他的操作。
- false：同步处理请求；异步对象必须处理请求，从服务端获取数据后，才能执行send之后的代码；任意时刻只能执行一个异步请求。

# JQuery

### 1、$是函数名

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script type="text/javascript">
			//单机按钮获取 文本框的 value值
			function fun1(){
				//通过js中的 id 获取 dom 对象
				// var obj = document.getElementById("txt1");
				var obj = $("txt1");
				alert(obj.value);
			}
			function fun2(){
				//通过js中的 id 获取 dom 对象
				// var obj = document.getElementById("txt2");
				var obj = $("txt2");
				alert(obj.value);
			}
			
			//自定义方法，获取js中的dom对象,$ 是一个函数的名称
			function $(domId){
				var obj = document.getElementById(domId);
				return obj;
			}
		</script>
	</head>
	<body>
		<input type="text" id="txt1" value="我是txt1" /><br>
		<input type="text" id="txt2" value="我是txt2" /><br>
		<input type="button" value="单击按钮 1" onclick="fun1()" /><br>
		<input type="button" value="单击按钮 2" onclick="fun2()" />
	</body>
</html>
```

### 2、JQuery简介

- jQuery是是一款跨主流浏览器的 JavaScript 库，封装了 JavaScript 相关方法调用，简化 JavaScript 对 HTML DOM 操作
- 库：相当于java的工具类，库是存放东西的， jQuery是存放js代码的地方， 放的用js代码写的function
- 官网首页介绍：jQuery 是一个快速，小巧，功能丰富的 JavaScript 库。 它通过易于使用的 API 在大 量浏览器中运行，使得 HTML 文档遍历和操作，事件处理，动画和 Ajax 变得更加简单。 通 过多功能性和可扩展性的结合，jQuery 改变了数百万人编写 JavaScript 的方式。

### 3、JQuery优点

- 写少代码，做多事情【write less do more】
- 免费，开源且轻量级的 js 库，容量很小
- 兼容市面上主流浏览器，例如 IE，Firefox，Chrome
- 能够处理 HTML、JSP、XML、CSS、DOM、事件、实现动画效果，也能提供异步 AJAX 功能
- 文档手册很全，很详细；https://jquery.cuishifeng.cn/
- 成熟的插件可供选择，多种 js 组件，例如日历组件（点击按钮显示下来日期）
- 出错后，有一定的提示信息
- 不用再在 html 里面通过<script>标签插入一大堆js来调用命令了

| JavaScript                      | JQuery        | 描述            |
| ------------------------------- | ------------- | --------------- |
| document.getElementById()       | $("#id名")    | 通过 ID 属性    |
| getElementsByClassName()        | $(".class名") | 通过 class 属性 |
| document.getElementsByTagName() | $("标签名")   | 通过标签名      |

### 4、JQuery下载

**官网下载地址**：https://jquery.com/download/

![image-20220115151319711](05JQuery.assets\image-20220115151319711.png)

### 5、DOM对象、JS对象和JQuery对象

- DOM对象：

  文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展标志语言的标准编程接口。 通过 DOM 对 HTML 页面的解析，可以将页面元素解析为元素节点、属性节点和文本节点，这些解析出的节点对象，即 DOM 对象。DOM 对象可以使用 JavaScript 中的方法。

  ```javascript
  //使用JavaScript的语法创建的对象叫做dom对象，也就是js对象
  var obj = document.getElementById("txt1");
  ```

- JavaScript对象：

  用 JavaScript 语法创建的对象叫做 JavaScript 对象, JavaScript 对象只能调用 JavaScript 对象的 API。

- JQuery对象：

  用 JQuery 语法创建的对象叫做 JQuery 对象, jQuery 对象只能调用 jQuery 对象的 API。 jQuery 对象是一个数组。在数组中存放本次定位的 DOM 对象。

  ```javascript
  //使用JQuery语法表示的对象叫做JQuery对象，注意：jQuery表示的对象都是数组
  var jobj = $("#txt1");
  //jobj就是使用jQuery语法表示的对象，也就是JQuery对象，它是一个数组，现在数组中就是一个值
  ```

- JQuery 对象与 JavaScript 对象是可以互相转化的，一般地，由于 Jquery 用起来更加方便， 我们都是将 JavaScript 对象转化成 Jquery 对象

  ```
  //dom --> jquery
  $(dom对象)
  //jquery --> dom
  从数组中获取第一个对象，第一个对象就是dom值，使用[0]或者get(0)
  
  为什么要进行dom和jQuery的转换：目的是要使用对象的方法，或者方法
  当你的dom对象时，可以使用dom对象的属性或者方法，如果你要想使用jquery提供的函数，必须是jquery对象才可以。
  ```

### 6、JQuery学习

##### 6.1 JQuery第一个例子s

```javascript
//使用 jQuery，首先要将 jQuery 库引入。使用如下语句：
<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JQuery第一个例子</title>
		<!--指定jQuery的库文件位置，使用相对路径，当前项目的js目录下的指定文件-->
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			/* 
				1.$(document),$是jQuery中的函数名称，document是函数的参数
					作用是document对象变成jQuery函数库可以使用的对象。 
				2.ready：是jQuery中的函数，是准备的意思，当页面的dom对象加载成功后
					会执行ready函数的内容。ready 相当于js中的onLoad事件
				3.function()自定义的表示onLoad后要执行的功能。
			*/
			// $(document).ready(function(){
			// 	alert("Hello Jquery");
			// })
			
			//简化
			$(function(){
				alert("Hello Jquery");
			})
            /*
            $(document).ready()与$()、jQuery()、window.jQuery()是等价的，
            所以$(document).ready()可以写成 $(function() { alert(“Hello jQuery”) } )
            */
		</script>
	</head>
	<body>
	</body>
</html>
```

##### 6.2 DOM对象转为JQuery对象

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>dom对象转为JQuery</title>
		<script type="text/javascript" src="./js/jquery-3.6.0.js"></script>
		<script>
			function btnClick(){
				//获取dom对象
				var obj = document.getElementById("btn");
				//使用dom的value属性,获取值
				alert("使用dom对象的属性="+obj.value)
							
				//把dom对象转jquery,使用jquery库中的函数
				var jobj = $(obj);
				//调用jquery中的函数,获取value的值
				alert( jobj.val() )
			}
		</script>
	</head>
	<body>
		<input type="button" id="btn" value="我是按钮" onclick="btnClick()" />
	</body>
</html>
```

##### 6.3 JQuery对象转为DOM对象

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script>
			function btnClick(){
				//使用JQuery语法获取页面中的dom对象
				var obj = $("#txt")[0];
				var num = obj.value;
				obj.value = num * num;
			}
		</script>
	</head>
	<body>
		<input type="button" value="计算平方" onclick="btnClick()" /><br />
		<input type="text" id="txt" value="整数" />
	</body>
</html>
```

##### 6.4 基本选择器

选择器: 就是定位条件；通知 jquery 函数定位满足条件的 DOM 对象，从而可以通过jquery的函数操作dom

1. id 选择器：语法：$("#dom对象的id名")

   通过dom对象的id定位dom对象的。 通过id找对象， id在当前页面中是唯一值。

2. class选择器：语法：$(".class样式名")

   class表示css中的样式， 使用样式的名称定位dom对象的。

3. 标签选择器：语法：$("标签名称")

   使用标签名称定为dom对象的

4. 所有选择器：语法：$("*")  选取页面中所有 DOM 对象。

5. 组合选择器：组合选择器是多个被选对象间使用逗号分隔后形成的选择器，可以组合 id，class，标签名等。

   语法：$("id选择器,class选择器,标签选择器")

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			div{
				background: gray;
				width: 200px;
				height: 100px;
			}
			
			.two{
				background: gold;
				font-size: 20pt;
			}
		</style>
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script>
			function fun1(){
				//id选择器
				var obj = $("#one");
				//使用jquery中改变样式的函数
				obj.css("background", "red");
			}
			
			function fun2(){
				//使用样式(class)选择器
				var obj = $(".two");
				obj.css("background","yellow");
			}
			
			function fun3(){
				//标签选择器
				var obj = $("div"); //数组有3个对象
				//jquery的操作都是操作数组中的全部成员.
				//所以是给所有的div都设置的背景色
				obj.css("background","blue");
			}
			
			function fun4(){
				var obj = $("*");
				obj.css("background","green");
			}
			
			function fun5(){
				var obj = $("#one,span");
				//obj.css("background","red");
							
				//obj是一个数组, 有两个成员, 1 是span dom对象
				//$(  obj[1] ) : jquery对象
				// $( dom 对象) : 是把dom对象转为jquery对象, 之后就可以调用jquery的css函数了
				$(obj[1]).css("background","green");//就是span				
			}
			
		</script>
	</head>
	<body>
		<div id="one">我是one的div</div><br/>
		<div class="two">我是样式是two的div</div><br/>
		<div>我是没有id，class的div</div><br/>
		<span class="two">我是span标签</span><br/>
		<input type="button" value="获取id是one的dom对象" onclick="fun1()" /><br/>
		<input type="button" value="使用class样式获取dom对象" onclick="fun2()" /><br/>
		<input type="button" value="使用标签选择器" onclick="fun3()" /> <br/>
		<input type="button" value="所有选择器" onclick="fun4()"/><br/>
		<input type="button" value="组合选择器" onclick="fun5()"/>
	</body>
</html>
```

##### 6.5 表单选择器

表单相关元素选择器是指文本框、单选框、复选框、下拉列表等元素的选择方式。该方法无论是否存在表单，均可做出相应选择。表单选择器是为了能更加容易地操作表单， 表单选择器是根据元素类型来定义的。

- <input type="text">
- <input type="password">
- <input type="radio">
- <input type="checkbox">
- <input type="button">
- <input type="file">
- <input type="submit">
- <input type="reset">

语法格式：$(":type 属性值")

- $(":text") 选取所有的单行文本框
- $(":password") 选取所有的密码框
- $(":radio") 选取所有的单选框
- $(":checkbox") 选取所有的多选框
- $(":file") 选取所有的上传按钮

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>表单选择器</title>
		<style type="text/css">
			
		</style>
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			function fun1(){
				//使用表单选择器 $(":type的值")
				var obj = $(":text");
				//获取value属性的值 val()是jquery中的函数, 读取value属性值
				alert( obj.val());
			}
			
			function fun2() {
				//定位radio
				var obj = $(":radio");//数组,目前是两个对象 man ,woman
				//循环数组,数组中的成员是dom对象, 可以dom的属性或者函数
				for(var i=0;i<obj.length;i++){
					//从数组值获取成员,使用下标的方式
					var dom = obj[i];
					//使用dom对象的属性,获取value值
					alert(dom.value)
				}
			}
			
			function fun3(){
				//定位checkbox
				var obj = $(":checkbox"); //数组,有三个对象
				for(var i=0;i<obj.length;i++){
					var dom = obj[i];
					//alert(dom.value);
					//使用jqueyr的val函数, 获取value的值
					//1. 需要jquery对象
					var jObj = $(dom); // jObj 是jquery对象
					//2. 调用jquery函数
					alert("jquery的函数调用=" + jObj.val());
				}
			}
		</script>
	</head>
	<body>
		<input type="text" value="我是type=text" /><br/>
		<br/>
		<input type="radio" value="man" /> 男 <br/>
		<input type="radio" value="woman" /> 女 <br/>
		<br/>
		<input type="checkbox" value="bike" /> 骑行 <br/>
		<input type="checkbox" value="football" /> 足球 <br/>
		<input type="checkbox" value="music" /> 音乐 <br/>
		<br/>
		<input type="button" value="读取text的值" onclick="fun1()"/>
		<br/>
		<input type="button" value="读取radio的值" onclick="fun2()"/>
		<br/>
		<input type="button" value="读取checkbox的值" onclick="fun3()"/>
	</body>
</html>
```

##### 6.6 基本过滤器

过滤器：在定位了dom对象后，根据一些条件筛选dom对象。

过滤器又是一个字符串，用来筛选dom对象的。

过滤器不能单独使用， 必须和选择器一起使用。

```
<div>1</div> dom1
<div>2</div> dom2
<div>3</div> dom3
$("div") == [dom1,dom2,dom3]
```

**基本过滤器**：

- $("选择器:first")：第一个dom对象
- $("选择器:last")：数组中的最后一个dom对象
- $("选择器:eq(数组的下标)")：获取指定下标的dom对象
- $("选择器:lt(下标)")：获取小于下标的所有dom对象
- $("选择器:gt(下标)")：获取大于下标的所有dom对象

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style type="text/css">
			div{
				background: gray;
			}
		</style>
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			// $(document).ready( 函数 ): 当页面中的dom对象加载成功后,会执行ready(), 
			// 相当于是onLoad().
			$(function() {
				//当页面dom对象加载后,给对象绑定事件,因为此时button对象已经在内存中创建好了.才能使用.
				 $("#btn1").click(function(){
					//过滤器
					var obj = $("div:first");
					obj.css("background","red");
				}) 
				
				//绑定事件
				$("#btn2").click(function(){
					var obj = $("div:last");
					obj.css("background","green");
				})
				
				//绑定btn3的事件
				$("#btn3").click(function(){
					var obj = $("div:eq(3)");
					obj.css("background","blue");
				})
				
				$("#btn4").click(function(){
					var obj = $("div:lt(3)");
					obj.css("background","orange");
				})
				
				$("#btn5").click(function(){
					var obj = $("div:gt(3)");
					obj.css("background","yellow");
				})
				
				$("#txt").keydown(function(){
					alert("keydown")
				})
			})
		</script>
	</head>
	<body>
		<input type="text" id="txt" />
		<div id="one">我是div-0</div>
		<div id="two">我是div-1</div>
		<div>我是div-2
		    <div>我是div-3</div>
			<div>我是div-4</div>
		</div>
		<div>我是div-5</div>
		<br />
		<span>我是span</span>
		
		<br/>
		<input type="button" value="获取第一个div" id="btn1"/>
		<br/>
		<input type="button" value="获取最后一个div" id="btn2"/>
		<br/>
		<input type="button" value="获取下标等于3的div" id="btn3"/>
		<br/>
		<input type="button" value="获取下标小于3的div" id="btn4"/>
		<br/>
		<input type="button" value="获取下标大于3的div" id="btn5"/>
	</body>
</html>
```

##### 6.7 表单属性过滤器

根据表单中dom对象的状态情况，定位dom对象的。

- $(":text:enabled")   选择可用的文本
- $(":text:disabled")  选择不可用的文本
- $(":checkbox:checked")  复选框选中的元素
- 选择器>option:selected   选择指定下拉列表的被选中元素

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			// $(document).ready( 函数 ): 当页面中的dom对象加载成功后,会执行ready(), 
			// 相当于是onLoad().
			$(function() {
				//当页面dom对象加载后,给对象绑定事件,因为此时button对象已经在内存中创建好了.才能使用.
				 $("#btn1").click(function(){
					//获取所有可以使用的text
					var obj  = $(":text:enabled");
					//设置 jquery数组值所有dom对象的value值
					obj.val("hello");
				}) 
				
				$("#btn2").click(function(){
					//获取选中的checkbox
					var obj  = $(":checkbox:checked");
					for(var i=0;i<obj.length;i++){
						//alert( obj[i].value);
						alert(    $(obj[i]).val()  ) 
					}
				})
				
				$("#btn3").click(function(){
					//获取select选中的值
					//var obj= $("select>option:selected");
					var obj = $("#yuyan>option:selected");
					alert(obj.val());
				})
			})
		</script>
	</head>
	<body>
		<input type="text"  id="txt1" value="text1" /><br/>
		<input type="text"  id="txt2" value="text2" disabled="true"/><br/>
		<input type="text"  id="txt3" value="text3" /><br/>
		<input type="text"  id="txt4" value="text4" disabled/><br/>
		<br/>
		<input type="checkbox" value="游泳" />游泳 <br/>
		<input type="checkbox" value="健身" checked />健身 <br/>
		<input type="checkbox" value="电子游戏" checked />电子游戏 <br/>
		<br/>
		<select id="yuyan">
			<option value="java">java语言</option>
			<option value="go" selected>go语言</option>
			<option value="python">python语言</option>
		</select>
	
		<br/><br/>
		<input type="button" value="设置可以的text的value是hello" id="btn1"/>
		<br/>
		<button id="btn2">显示选中的复选框的值</button>
		<br/>
		<button id="btn3">显示选中下拉列表框的值</button>
	</body>
</html>
```

##### 6.8 函数

1. val

   操作数组中 DOM 对象的 value 属性；

   $(选择器).val() ：无参数调用形式，读取数组中第一个 DOM 对象的 value 属性值 

   $(选择器).val(值)：有参形式调用；对数组中所有 DOM 对象的 value 属性值进行统一赋值

2. text

   操作数组中所有 DOM 对象的【文字显示内容属性】

   $(选择器).text():无参数调用，读取数组中所有 DOM 对象的文字显示内容，将得到内容拼接为一个字符串返回

   $(选择器).text(值):有参数方式，对数组中所有 DOM 对象的文字显示内容进行统一赋值

3. attr

   对 val, text 之外的其他属性操作

   $(选择器).attr(“属性名”): 获取 DOM 数组第一个对象的属性值

   $(选择器).attr(“属性名”,“值”): 对数组中所有 DOM 对象的属性设为新值

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<style type="text/css">
   			div{
   				background: yellow;
   			}
   		</style>
   		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
   		<script type="text/javascript">
   			//在dom对象创建好后,绑定事件
   			$(function(){
   				$("#btn1").click(function(){
   					//val() 获取dom数组中第一个对象的value属性值
   					var text = $(":text").val();
   					alert(text) // 刘备
   				})
   				
   				$("#btn2").click(function(){
   					//设置所有的text的value为新值
   					$(":text").val("三国演义");
   				})
   				
   				$("#btn3").click(function(){
   					//获取div ,text()无参数,获取dom对象的文本值,连接成一个字符串
   					alert($("div").text()); 
                         // 1.我第一个div2.我第二个div3.我第三个div返回顶部
   				})
   				
   				$("#btn4").click(function(){
   					//设置div的文本值
   					$("div").text("新的div文本内容");
   				})
   				
   				$("#btn5").click(function(){
   					//读取指定属性的值
   					alert($("img").attr("src"));// img/ex1.jpg
   				})
   				
   				$("#btn6").click(function(){
   					//设置指定属性的,指定值
   					$("img").attr("src","img/ex2.jpg");
   					//val(), text();
   				})
   			})			
   		</script>
   	</head>
   	<body>
   		<input type="text" value="刘备" /><br/>
   		<input type="text" value="关羽" /><br/>
   		<input type="text" value="张飞" /><br/>
   		<br/>
   		<div>1.我第一个div</div>
   		<div>2.我第二个div</div>
   		<div>3.我第三个div</div>
   		<br/>
   		<img src="img/ex1.jpg" id="image1" />
   		<br/>
   		
   		<input type="button" value="获取第一文本框的值" id="btn1"/>
   		<br/>
   		<br/>
   		<input type="button" value="设置所有文本框的value值" id="btn2"/>
   		<br/>
   		<br/>
   		<input type="button" value="获取所有div的文本值" id="btn3"/>
   		<br/>
   		<br/>
   		<input type="button" value="设置div的文本值" id="btn4"/>
   		<br/>
   		<br/>
   		<input type="button" value="读取src属性的值" id="btn5"/>
   		<br/>
   		<br/>
   		<input type="button" value="设置指定的属性值" id="btn6"/>
   	</body>
   </html>
   ```

4. hide

   $(选择器).hide() : 将数组中所有 DOM 对象隐藏起来

5. show

   $(选择器).show():将数组中所有 DOM 对象在浏览器中显示起来

6. remove

   $(选择器).remove() : 将数组中所有 DOM 对象及其子对象一并删除

7. empty

   $(选择器).empty()：将数组中所有 DOM 对象的子对象删除

8. append

   为数组中所有 DOM 对象添加子对象

   $(选择器).append("<div>我动态添加的 div</div>")

9. html

   设置或返回被选元素的内容（innerHTML）。

   $(选择器).html()：无参数调用方法，获取 DOM 数组第一个匹元素的内容。

   $(选择器).html(值)：有参数调用，用于设置 DOM 数组中所有元素的内容。

10. each

    each 是对数组、json 和 dom 数组等的遍历,对每个元素调用一次函数。

    语法 1：$.each( 要遍历的对象, function(index,element) { 处理程序 } )

    语法 2：jQuery 对象.each( function( index, element ) { 处理程序 } )

    index: 数组的下标 

    element: 数组的对象

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style type="text/css">
			div{
				background: yellow;
			}
		</style>
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			//在dom对象创建好后,绑定事件
			$(function(){
				$("#btn1").click(function(){
					//使用remove:删除父和子所有的dom对象
					$("select").remove();
				})
				
				$("#btn2").click(function(){
					//使用empty 删除子dom对象
					$("select").empty();
				})
				
				$("#btn3").click(function(){
					//使用append,增加dom对象
					// $("#fatcher").append("<input type='button' value='我是增加的按钮' />")
					//增加一个table
					$("#fatcher").append("<table border=1><tr><td>第一列</td><td>第二列</td></tr></table>");
				})
				
				$("#btn4").click(function(){
					//使用html()函数,获取数组中第一个dom对象的文本值(innerHTML)
					alert($("span").text()); // 我是mysql 数据库我是jdbc
					alert( $("span").html() ); //我是mysql <b>数据库</b>
				})
				
				$("#btn5").click(function(){
					//使用 html(有参数):设置dom对象的文本值
					$("span").html("我是新的<b>数据</b>");
				})
				
				$("#btn6").click(function(){
					//循环普通数组,非dom数组
					var  arr = [ 11, 12, 13];
					$.each(arr, function(i,n){
						alert("循环变量："+i + "=====数组成员:"+ n);
						//循环变量：0=====数组成员:11
						//循环变量：1=====数组成员:12
						//循环变量：2=====数组成员:13
					})
				})
				
				$("#btn7").click(function(){
					//循环json
					var json={"name":"张三","age":20};
					//var obj = eval("{'name':'张三','age':20}");
					$.each(json,function(i,n){
						alert("i是key="+i+",n是值="+n);
						//i是key=name,n是值=张三
						//i是key=age,n是值=20
					})
				})
				
				$("#btn8").click(function(){
					//循环dom数组
					var domArray = $(":text");//dom数组
					$.each(domArray, function(i,n){
						// n 是数组中的dom对象
						alert("i="+i+"  , n="+n.value);
						//i=0  , n=刘备
						//i=1  , n=关羽
						//i=2  , n=张飞
					})
				})
				
				$("#btn9").click(function(){
					//循环jquery对象, jquery对象就是dom数组
					$(":text").each(function(i,n){
						alert("i="+i+"，n="+ n.value);
						//i=0  , n=刘备
						//i=1  , n=关羽
						//i=2  , n=张飞
					})
				})
			})
		</script>
	</head>
	<body>
		<input type="text" value="刘备" />
		<input type="text" value="关羽" />
		<input type="text" value="张飞" />
		
		<br/>
		<select>
			<option value="老虎">老虎</option>
			<option value="狮子">狮子</option>
			<option value="豹子">豹子</option>
		</select>
		<br/>
		<br/>
		<select>
			<option value="亚洲">亚洲</option>
			<option value="欧洲">欧洲</option>
			<option value="美洲">美洲</option>
		</select>
		<br/>
		<br/>
		<div id="fatcher">我是第一个div</div>
		<br/
		<br/>
		<span>我是mysql <b>数据库</b></span>
		<br/>
		<span>我是jdbc</span>
		<br/>
		<br/>
		
		<input type="button" value="使用remove删除父和子对象" id="btn1"/>
		<br/>
		<br/>
		<input type="button" value="使用empty删子对象" id="btn2"/>
		<br/>
		<br/>
		<input type="button" value="使用append,增加dom对象" id="btn3"/>
		<br/>
		<br/>
		<input type="button" value="获取第一个dom的文本值" id="btn4"/>
		<br/>
		<br/>
		<input type="button" value="设置span的所以dom的文本值" id="btn5"/>
		<br/>
		<br/>
		<input type="button" value="循环普通数组" id="btn6"/>
		<br/>
		<br/>
		<input type="button" value="循环json" id="btn7"/>
		<br/>
		<br/>
		<input type="button" value="循环dom数组" id="btn8"/>
		<br/>
		<br/>
		<input type="button" value="循环jquery对象" id="btn9"/>
	</body>
</html>
```

##### 6.9 JQuery绑定事件方式

1. 定义元素监听事件

   $(选择器).事件名称( 事件的处理函数)
   $(选择器)：定位dom对象， dom对象可以有多个， 这些dom对象都绑定事件了
   事件名称：就是js中事件去掉on的部分， 例如 js中的单击事件 onclick(),
   	             jquery中的事件名称，就是click，都是小写的。
   事件的处理函数：就是一个function ，当事件发生时，执行这个函数的内容。

   ```
   例如给id是"btn"的按钮绑定单击事件
   $("#btn").click(funtion(){
   	alert("btn按钮单击了")
   })
   ```

2. on() 绑定事件

   on() 方法在被选元素上添加事件处理程序。该方法给 API 带来很多便利，推荐使用该方法。

   语法：$(选择器).on(event, data, function)

   event：事件一个或者多个，多个之间空格分开

   data：可选。规定传递到函数的额外数据，json 格式

   function: 可选。规定当事件发生时运行的函数。

   ```javascript
    <input type="button" id="btn">
   $("#btn").on("click", function() { 
        //处理按钮单击 
   })
   ```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style type="text/css">
			div{
				background: yellow;
			}
		</style>
		<script type="text/javascript" src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			//在dom对象创建好后,绑定事件
			$(function(){
				$("#btn1").click(function(){
					//使用append增加dom对象
					$("#mydiv").append("<input id='newBtn' type='button' value='我是新加入的按钮'/>");
					//使用on给按钮绑定事件
					$("#newBtn").on("click",function(){
						alert("新建的按钮被单击了");
					})
				})
			})
		</script>
	</head>
	<body>
		<div id="mydiv">
			我是一个div ，需要增加一个button
		</div>
		<input type="button" value="创建一个button,绑定一个click" id="btn1"/>
		<br/>
	</body>
</html>
```

##### 6.10 AJAX语法

- 没有jquery之前，使用XMLHttpRequest做ajax，有4个步骤。jquery简化了ajax请求的处理。
- 使用三个函数可以实现ajax请求的处理。
  - $.ajax()：jquery中实现ajax的核心函数。
  - $.post()：使用post方式做ajax请求。
  - $.get()：使用get方式发送ajax请求。
  - $.post() 和 $.get() 他们在内部都是调用的 $.ajax() 

1. $.ajax()函数的使用

   $.ajax() 是 jQuery 中 AJAX 请求的核心方法，所有的其他方法都是在内部使用此方法。

   语法：$.ajax( { name:value, name:value, ... } )

   说明：参数是 json 的数据，包含请求方式，数据，回调方法等

   参数：

   - async：布尔值，表示请求是否异步处理。默认是 true（异步）

   - contentType：发送数据到服务器时所使用的内容类型。默认是："application/x-www-form-urlencoded"

     例如你想表示请求的参数是json格式的， 可以写："application/json"

   - data：规定要发送到服务器的数据，可以是：string，数组，多数是 json

   - dataType：**期望**从服务器端响应的数据类型。jQuery 从 xml, json, text,, html 这些中测试最可能的类型

     - "xml"：一个 XML 文档
     - "html"：HTML 作为纯文本
     - "text"：纯文本字符串
     - "json"：以 JSON 运行响应，并以对象返回

   - error(xhr,status,error)：如果请求失败要运行的函数，其中 xhr, status, error 是自定义的形参名

     ```
     error:function() {   发生错误时执行  }  
     ```

   - success(result,status,xhr)：当请求成功时运行的函数，其中 result, status, xhr 是自定义的形参名

     ```
     sucess:一个function，请求成功了，从服务器端返回了数据，会执行success指定函数
            之前使用XMLHttpRequest对象， 当readyState==4 && status==200的时候。
     ```

   - type：规定请求的类型（GET 或 POST 等），默认是 GET；get，post 不用区分大小写

   - url：规定发送请求的 URL

   **注意：error() , success()中的 xhr 是 XMLHttpRequest 对象**

   ```js
   $.ajax({ 
   	async:true , 
   	contentType:"application/json" , 
   	data: {name:"lisi",age:20 },
   	dataType:"json",
   	error:function(){
           请求出现错误时，执行的函数
   	},
   	success:function( data ) {
           //data就是responseText, 是jquery处理后的数据。
   	},
   	url:"bmiAjax",
   	type:"get"
   })
   ```

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: Amadeus
     Date: 2022/1/10
     Time: 14:30
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>使用JSON格式的数据</title>
       <script type="text/javascript" src="/js/jquery-3.6.0.js"></script>
       <script type="text/javascript">
           $(function (){
               $("#btn").click(function (){
                   //获取dom的value值
                   var proid = $("#proid").val();
                   //发起ajax请求
                   $.ajax({
                       url:"queryJson",
                       data:{"proid": proid},
                       dataType:"json",
                       success:function (resp){
                           $("#proname").val(resp.name);
                           $("#prosh").val(resp.shenghui);
                           $("#projc").val(resp.jiancheng);
                       }
                   })
               })
           })
       </script>
   </head>
   <body>
       <p>ajax请求使用json格式的数据</p>
       <table border="2">
           <tr>
               <td>省份编号</td>
               <td>
                   <input type="text" id="proid">
                   <input type="button" value="搜索" id="btn">
               </td>
           </tr>
           <tr>
               <td>省份名称</td>
               <td><input type="text" id="proname"></td>
           </tr>
           <tr>
               <td>省份简称</td>
               <td><input type="text" id="projc"></td>
           </tr>
           <tr>
               <td>省会名称</td>
               <td><input type="text" id="prosh"></td>
           </tr>
       </table>
   </body>
   </html>
   ```

2. $.get()

   $.get() 方法使用 HTTP GET 请求从服务器加载数据。

   语法：$.get(url,data,function(data,status,xhr),dataType)

   - url：必需。规定您需要请求的 URL。

   - data：可选。规定连同请求发送到服务器的数据。

   - function(data,status,xhr) 可选。当请求成功时运行的函数。data,status,xhr 是自定义形参名。

     - data：包含来自请求的结果数据
     - status：包含请求的状态（"success"、"notmodified"、"error"、"timeout"、"parsererror"）
     - xhr：包含 XMLHttpRequest 对象

   - dataType：可选。规定预期的服务器响应的数据类型。默认地，jQuery 会智能判断。

     可能的类型：

     - "xml"：一个 XML 文档 
     - "html"：HTML 作为纯文本 
     - "text"：纯文本字符串	
     - "json" - 以 JSON 运行响应，并以对象返回

3.  $.post()

   $.post() 方法使用 HTTP POST 请求从服务器加载数据。

   语法：$.post(URL,data,function(data,status,xhr),dataType)

   参数同$get()

### 7、级联查询功能

##### 7.1 准备

**数据库**：

- province:

  ![image-20220116171816686](05JQuery.assets\image-20220116171816686.png)

- city:

  ![image-20220116171851203](05JQuery.assets\image-20220116171851203.png)

JAVA实体类：

```java
public class Province {
    private Integer id;
    private String name;
    private String jiancheng;
    private String shenghui;
    
    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getJiancheng() {
        return jiancheng;
    }
    public void setJiancheng(String jiancheng) {
        this.jiancheng = jiancheng;
    }
    public String getShenghui() {
        return shenghui;
    }
    public void setShenghui(String shenghui) {
        this.shenghui = shenghui;
    }
    @Override
    public String toString() {
        return "Province{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", jiancheng='" + jiancheng + '\'' +
                ", shenghui='" + shenghui + '\'' +
                '}';
    }
}
```

```java
public class City {
    private Integer id;
    private String name;
    private Integer provinceId;

    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Integer getProvinceId() {
        return provinceId;
    }
    public void setProvinceId(Integer provinceId) {
        this.provinceId = provinceId;
    }
    @Override
    public String toString() {
        return "City{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", provinceId=" + provinceId +
                '}';
    }
}
```

##### 7.2 查询所有的省份信息

```java
public class QueryProvinceServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String json = "{}";
        //调用Dao，获取所有的省份信息，是一个List集合
        List<Province> provinceList = QueryDao.queryProvinceList();
        //把List转为一个JSON格式的数据，输出给ajax请求
        if(provinceList != null && provinceList.size() != 0){
            //调用jackson工具库，实现list --> json
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(provinceList);
        }
        //输出json数据，响应ajax请求的，返回数据
        response.setContentType("application/json;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.println(json);
        pw.flush();
        pw.close();
    }
}
```

```java
public class QueryDao {
    //查询所有的省份信息
    public static List<Province> queryProvinceList(){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        Province province = null;
        List<Province> provinceList = new ArrayList<>();
        try {
            String sql = "select id, name, jiancheng, shenghui from province order by id";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            rs = ps.executeQuery();
            while(rs.next()){
                province = new Province();
                province.setId(rs.getInt("id"));
                province.setName(rs.getString("name"));
                province.setJiancheng(rs.getString("jiancheng"));
                province.setShenghui(rs.getString("shenghui"));
                provinceList.add(province);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return provinceList;
    }
}
```

##### 7.3 查询一个省份下面所有的城市

```java
public class QueryCityServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求传过来的省份ID
        String proid = request.getParameter("proid");
        String json = "{}";
        if(proid != null && !"".equals(proid)){
            List<City> cityList = QueryDao.queryCityList(Integer.valueOf(proid));
            //list --> json
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(cityList);
        }
        //输出json数据，响应ajax请求的，返回数据
        response.setContentType("application/json;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.println(json);
        pw.flush();
        pw.close();
    }
}
```

```java
public class QueryDao {
    //查询一个省份下面所有的城市
    public static List<City> queryCityList(Integer provinceId){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        City city = null;
        List<City> cityList = new ArrayList<>();
        try {
            String sql = "select id, name from city where provinceId = ?";
            conn = JdbcUtil.getConnection();
            ps = conn.prepareStatement(sql);
            ps.setInt(1, provinceId);
            rs = ps.executeQuery();
            while(rs.next()){
                city = new City();
                city.setId(rs.getInt("id"));
                city.setName(rs.getString("name"));
                cityList.add(city);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtil.close(conn, ps, rs);
        }
        return cityList;
    }
}
```

##### 7.4 index.jsp页面

```jsp
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <title>省市级联查询</title>
    <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
    <script type="text/javascript">
        function loadDataAjax() {
            //做ajax请求，使用jquery的$.ajax()
            $.ajax({
                url:"queryProvince",
                dataType:"json",
                success:function( resp ){
                    //删除旧的数据，把已经存在的数据清空
                    $("#province").empty();
                    //[{"id":1,"name":"河北","jiancheng":"冀","shenghui":"石家庄"},{}]
                    $.each( resp, function (i,n) {
                        //获取select这个dom对象
                        $("#province").append("<option value='"+n.id+ "'>" +  n.name + "</option>");
                    })
                }
            })
        }

        $(function(){
            //  $(function()）在页面的dom的对象加载成功后执行的函数， 在此发起ajax。
            loadDataAjax();
            //绑定事件
            $("#btnLoad").click(function(){
                loadDataAjax();
            })
            //给省份的select绑定一个change事件，当select内容发生变化时，触发事件
            $("#province").change(function () {
                //获取选中的列表框的值
                var privinceId = $("#province>option:selected").val();
                //做一个ajax请求，获取省份的所有城市信息
                $.post("queryCity", { proid: privinceId }, function (resp){
                    //删除旧的数据，把已经存在的数据清空
                    $("#city").empty();
                    $.each(resp, function (index, value){
                        $("#city").append("<option value="+ value.id +">"+ value.name +"</option>")
                    })
                }, "json");
            })
        })
    </script>
</head>
<body>
<p>省市级联查询</p>
<div>
    <table>
        <tr>
            <td>省份：</td>
            <td>
                <select id="province">
                    <option value="0">请选择......</option>
                </select>
            </td>
            <td>
                <input type="button" value="load数据" id="btnLoad">
            </td>
        </tr>
        <tr>
            <td>城市：</td>
            <td>
                <select id="city">
                    <option value="0">请选择......</option>
                </select>
            </td>
        </tr>
    </table>
</div>
</body>
</html>
```

