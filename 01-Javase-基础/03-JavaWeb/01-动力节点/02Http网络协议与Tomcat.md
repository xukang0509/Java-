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

