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

