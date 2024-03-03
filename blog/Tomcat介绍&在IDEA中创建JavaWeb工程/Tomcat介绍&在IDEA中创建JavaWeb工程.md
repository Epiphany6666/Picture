在IDEA中创建JavaWeb工程

@[TOC]

# 一、WEB服务器-Tomcat

## 服务器概述

- 指的也是计算机，只不过服务器要比我们日常使用的计算机大很多。

![image-20221202173148317](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture@master/blog/%E5%9C%A8IDEA%E4%B8%AD%E5%88%9B%E5%BB%BAJavaWeb%E5%B7%A5%E7%A8%8B/assets/202403031527767.png) 

服务器，也称伺服器。是提供计算服务的设备。由于服务器需要响应服务请求，并进行处理，因此一般来说服务器应具备承担服务并且保障服务的能力。

服务器的构成包括处理器、硬盘、内存、系统总线等，和通用的计算机架构类似，但是由于需要提供高可靠的服务，因此在处理能力、稳定性、可靠性、安全性、可扩展性、可管理性等方面要求较高。

在网络环境下，根据服务器提供的服务类型不同，可分为：文件服务器，数据库服务器，应用程序服务器，WEB服务器等。

服务器只是一台设备，必须安装服务器软件才能提供相应的服务。

**服务器软件**

服务器软件：基于ServerSocket编写的程序

- 服务器软件本质是一个运行在服务器设备上的应用程序
- 能够接收客户端请求，并根据请求给客户端响应数据

![1530625192392](../../../../StudyNote/JavaWeb/assets/1530625192392.png)



#### Web服务器

Web服务器是一个应用程序(软件)，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作(不用程序员自己写代码去解析http协议规则)，让Web开发更加便捷。主要功能是"提供网上信息浏览服务"。

![](../../../../StudyNote/JavaWeb/assets/image-20220824233614686.png)

Web服务器是安装在服务器端的一款软件，将来我们把自己写的Web项目部署到Tomcat服务器软件中，当Web服务器软件启动后，部署在Web服务器软件中的页面就可以直接通过浏览器来访问了。

# Web服务器软件使用步骤

* 准备静态资源
* 下载安装Web服务器软件
* 将静态资源部署到Web服务器上
* 启动Web服务器使用浏览器访问对应的资源

**第1步：下载安装Web服务器软件**

![image-20221202181110555](../../../../StudyNote/JavaWeb/assets/image-20221202181110555.png)

**第2步：创建项目**

或者直接新建一个项目文件夹，这里的baidu就是context root

![image-20240303121023794](../../../../StudyNote/JavaWeb/assets/image-20240303121023794.png)

然后再在项目文件夹里新建 WEB-INF文件夹，这个文件夹不能随便命名

![image-20240303121115149](../../../../StudyNote/JavaWeb/assets/image-20240303121115149.png)

一个web项目最简单的形式就是两个文件夹

**第3步：将静态资源部署到Web服务器上**

然后将网页静态代码放在和WEB-INF同级的地方

![image-20240303121233942](../../../../StudyNote/JavaWeb/assets/image-20240303121233942.png)

**第4步：启动Web服务器使用浏览器访问对应的资源**

![image-20221202181346327](../../../../StudyNote/JavaWeb/assets/image-20221202181346327.png)

浏览器输入：`http://localhost:8080/baidu/index.html`

![image-20240303121450129](../../../../StudyNote/JavaWeb/assets/image-20240303121450129.png)

> 一个Tomcat中可能会有很多个项目，但是她们的context root不能一样，通过ip地址+端口号就能定位到某台服务器的端口号。通过context root就能定位到某个项目。然后将资源响应给客户端。

上述内容在演示的时候，使用的是Apache下的Tomcat软件，至于Tomcat软件如何使用，后面会详细的讲到。而对于Web服务器来说，实现的方案有很多，Tomcat只是其中的一种，而除了Tomcat以外，还有很多优秀的Web服务器，比如:

> Tomcat是轻量级的服务器，只支持少量的javaEE规范
>
> 而WebLogic、WebSphere支持全部的JavaEE规范，所以它们是重量级的服务器

![image-20220824233728524](../../../../StudyNote/JavaWeb/assets/image-20220824233728524.png) 

Tomcat就是一款软件，我们主要是以学习如何去使用为主。具体我们会从以下这些方向去学习:

1. 简介：初步认识下Tomcat

2. 基本使用: 安装、卸载、启动、关闭、配置和项目部署，这些都是对Tomcat的基本操作

3. IDEA中如何创建Maven Web项目

4. IDEA中如何使用Tomcat,后面这两个都是我们以后开发经常会用到的方式

首选我们来认识下Tomcat。





#### Tomcat

Tomcat服务器软件是一个免费的开源的web应用服务器。是Apache软件基金会的一个核心项目。由Apache，Sun和其他一些公司及个人共同开发而成。

由于Tomcat只支持Servlet/JSP少量JavaEE规范，所以是一个开源免费的轻量级Web服务器。

> JavaSE：java标准版Java SE（Java Platform，Standard Edition）
>
> JavaME：java小型版
>
> JavaEE规范：   JavaEE => Java Enterprise Edition(Java企业版)
>
> avaEE规范就是指Java企业级开发的技术规范总和。包含13项技术规范：JDBC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF

因为Tomcat支持Servlet/JSP规范，所以Tomcat也被称为Web容器（WebContainer）、Servlet容器（Servlet是基于Servlet规范开发出来的一种web资源）。Servlet程序需要依赖于支持Servlet这种Web服务器才可以运行。

JavaWeb程序需要依赖Tomcat才能运行。

Tomcat的官网: https://tomcat.apache.org/ 

![image-20220824233903517](../../../../StudyNote/JavaWeb/assets/image-20220824233903517.png) 

# 65.Tomcat——基本使用

#### 下载

直接从官方网站下载：https://tomcat.apache.org/download-90.cgi

![](../../../../StudyNote/JavaWeb/assets/image-20220824234407828.png)

> Tomcat软件类型说明：
>
> - tar.gz文件，是linux和mac操作系统下的压缩版本
> - zip文件，是window操作系统下压缩版本（我们选择zip文件）

大家可以自行下载，也可以直接使用资料中已经下载好的资源，

Tomcat的软件程序  ：/资料/SpringbootWeb/apache-tomcat-9.0.27-windows-x64.zip

> 建议不要下载10，因为tomcat的版本是和JDK配套的。

![](../../../../StudyNote/JavaWeb/assets/image-20220824234527743.png) 

 



**3.2.2 安装与卸载**

**安装:** Tomcat是绿色版，直接解压即安装

> 在E盘的develop目录下，将`apache-tomcat-9.0.27-windows-x64.zip`进行解压缩，会得到一个`apache-tomcat-9.0.27`的目录，Tomcat就已经安装成功。

![image-20221202184545321](../../../../StudyNote/JavaWeb/assets/image-20221202184545321.png)

==注意，Tomcat在解压缩的时候，解压所在的目录可以任意，但最好解压到一个不包含中文和空格的目录，因为后期在部署项目的时候，如果路径有中文或者空格可能会导致程序部署失败。==



打开`apache-tomcat-9.0.27`目录就能看到如下目录结构，每个目录中包含的内容需要认识下

> lib：tomcat本身也是用java和c写的程序，它本身也是一个项目，所以它本身也依赖一些jar包类。

![](../../../../StudyNote/JavaWeb/assets/image-20220824234652173.png)  

bin：目录下有两类文件，一种是以`.bat`结尾的，是Windows系统的可执行文件，一种是以`.sh`结尾的，是Linux系统的可执行文件。

webapps：就是以后项目部署的目录



**卸载：**卸载比较简单，可以直接删除目录即可





**3.2.3 启动与关闭**

**启动Tomcat**

> tomcat也是用java和C写的，所以它也需要有java虚拟机（Java的运行环境），因此需要告诉tomcat，当前电脑的JDK装在什么地方。配置JAVA_HOME即可。

- 双击tomcat解压目录/bin/**startup.bat**文件即可启动tomcat

![image-20221202183201663](../../../../StudyNote/JavaWeb/assets/image-20221202183201663.png)

==注意: tomcat服务器启动后,黑窗口不会关闭,只要黑窗口不关闭,就证明tomcat服务器正在运行==

![image-20221202183409304](../../../../StudyNote/JavaWeb/assets/image-20221202183409304.png)

Tomcat的默认端口为8080，所以在浏览器的地址栏输入：`http://127.0.0.1:8080` 即可访问tomcat服务器

> 127.0.0.1 也可以使用localhost代替。如：`http://localhost:8080`

![image-20221202183550682](../../../../StudyNote/JavaWeb/assets/image-20221202183550682.png)

- 能看到以上图片中Apache Tomcat的内容就说明Tomcat已经启动成功

==注意事项== ：Tomcat启动的过程中，遇到控制台有中文乱码时，可以通常修改conf/logging.prooperties文件解决

![image-20220825083848086](../../../../StudyNote/JavaWeb/assets/image-20220825083848086.png) 



**关闭:**  关闭有三种方式 

1、强制关闭：直接x掉Tomcat窗口（不建议）

![image-20221202184753808](../../../../StudyNote/JavaWeb/assets/image-20221202184753808.png)

2、正常关闭：bin\shutdown.bat

![image-20221202185103941](../../../../StudyNote/JavaWeb/assets/image-20221202185103941.png)

3、正常关闭：在Tomcat启动窗口中按下 Ctrl+C

- 说明：如果按下Ctrl+C没有反映，可以多按几次





#### 常见问题

**问题1：Tomcat启动时，窗口一闪而过**

- 检查JAVA_HOME环境变量是否正确配置

  > 因为Tomcat中需要用到Java_Home这个环境变量

![image-20221202190033167](../../../../StudyNote/JavaWeb/assets/image-20221202190033167.png)



**问题2：端口号冲突**

![image-20220825084104447](../../../../StudyNote/JavaWeb/assets/image-20220825084104447.png)

- 发生问题的原因：Tomcat使用的端口被占用了。

- 解决方案：换Tomcat端口号
  - 要想修改Tomcat启动的端口号，需要修改 conf/server.xml文件

![image-20231026135055438](../../../../StudyNote/JavaWeb/assets/image-20231026135055438.png) 

> 注: HTTP协议默认端口号为80，如果将Tomcat端口号改为80，则将来访问Tomcat时，将不用输入端口号。
>
> localhost:80等价于localhost

# 在IDEA中将项目部署到tomcat中

## 新建Java Web项目

在父项目中新建子模块

![image-20240303122756456](../../../../StudyNote/JavaWeb/assets/image-20240303122756456.png)

添加web模块

- 方法一：

  > 旧版IDEA可以直接右击模块，然后选择Add framework support添加web应用程序即可。
  >
  > 新版IDEA 2023.2以上版本 没有Add framework support选项。
  >
  > 解决办法：选中模块，双击shift，选择操作，[中文版](https://so.csdn.net/so/search?q=中文版&spm=1001.2101.3001.7020)搜索添加框架支持，英文版搜索Add framework support，即可使用

  ![image-20240303124336819](../../../../StudyNote/JavaWeb/assets/image-20240303124336819.png)

  ![image-20240303124400158](../../../../StudyNote/JavaWeb/assets/image-20240303124400158.png)

  删除jsp文件

  ![image-20240303124516959](../../../../StudyNote/JavaWeb/assets/image-20240303124516959.png)

  

- 方法二：

  ![image-20240303144606663](../../../../StudyNote/JavaWeb/assets/image-20240303144606663.png)

  然后检查目录是否配置正确

  ![image-20240303144754205](../../../../StudyNote/JavaWeb/assets/image-20240303144754205.png)

  最后再点击应用，ok即可

  ![image-20240303144831605](../../../../StudyNote/JavaWeb/assets/image-20240303144831605.png)

  



在web这一级新建html文件

![image-20240303133616190](../../../../StudyNote/JavaWeb/assets/image-20240303133616190.png)

![image-20240303133728296](../../../../StudyNote/JavaWeb/assets/image-20240303133728296.png)

新建本地tomcat

![image-20240303134010846](../../../../StudyNote/JavaWeb/assets/image-20240303134010846.png)

配置tomcat

![image-20240303134543926](../../../../StudyNote/JavaWeb/assets/image-20240303134543926.png)

将项目部署到tomcat中

![image-20240303134621165](../../../../StudyNote/JavaWeb/assets/image-20240303134621165.png)

> WAR (Web ARchive) 文件是一种归档格式，用于将Java Web应用程序的所有组件（包括HTML文件、图像、Java类、JSP等）打包在一起。"war exploded"意味着你的项目已经被展开或解压缩到其各个部分，而不是作为一个单独的WAR文件。这种部署方式的优点是可以提高性能，因为服务器可以直接访问已展开的文件，而不需要先解压缩WAR文件。此外，某些IDE（如IntelliJ IDEA）可能会更轻松地进行热部署（hot deployment），即在运行时更新代码。
>
> 另一种选项 "pro01-javaweb-begin:Web exploded" 可能是指整个项目（包括源代码和其他资源）被展开并部署到了服务器上。这可能意味着你的项目没有被打包成WAR文件，而是直接部署了源代码和资源。
>
> 在实际应用中，通常推荐使用WAR文件进行部署，因为它更容易管理和维护。然而，在开发过程中，使用 "war exploded" 部署可以提供更快的反馈循环，因为更改可以立即反映在运行的应用程序中，而无需重新打包和部署WAR文件。

![image-20240303140810772](../../../../StudyNote/JavaWeb/assets/image-20240303140810772.png)

下面的`应用程序上下文`就是我们所说的context root

![image-20240303141142439](../../../../StudyNote/JavaWeb/assets/image-20240303141142439.png)

为了我们后面写代码比较方便，一般情况下直接改成一个 \ 

![image-20240303141303098](../../../../StudyNote/JavaWeb/assets/image-20240303141303098.png)

则网址上对应的项目名称就可以省略了

`http://localhost:8080/baidu/index.html `也就变成了  `http://localhost:8080/index.html`

修改tomcat启动时打开浏览器时自动跳转到的页面的URL

![image-20240303141537190](../../../../StudyNote/JavaWeb/assets/image-20240303141537190.png)

当有更新操作时，执行重新部署

![image-20240303141929714](../../../../StudyNote/JavaWeb/assets/image-20240303141929714.png)

当IDEA失去焦点后，重新更新类和资源

![image-20240303142038492](../../../../StudyNote/JavaWeb/assets/image-20240303142038492.png)

当在IDEA中修改tomcat端口时，修改的其实也是tomcat的config文件

![image-20240303142202405](../../../../StudyNote/JavaWeb/assets/image-20240303142202405.png)

然后应用，确认即可

建议养成习惯，运行的时候都点debug模式而不是运行模式。

好处是：如果出问题了，就可以直接设置断点，直接调试。如果是运行模式，断点就没啥用了。

![image-20240303143047631](../../../../StudyNote/JavaWeb/assets/image-20240303143047631.png)

tomcat运行成功，自动弹出浏览器窗口！

![image-20240303143322776](../../../../StudyNote/JavaWeb/assets/image-20240303143322776.png)

eclipse在配置tomcat，在部署的时候，它是正儿八经的把这个项目部署到webapp目录的，但idea是部署在pro-web\out\artifacts中，然后在tomcat中去指明，所部署的项目在这，所以原始的webapp目录基本上就没啥用了。

![image-20240303144127883](../../../../StudyNote/JavaWeb/assets/image-20240303144127883.png)



## 出现的问题

web目录前面没有蓝色小点

![image-20240303150652014](../../../../StudyNote/JavaWeb/assets/image-20240303150652014.png)

进入项目结构，然后跟着下图操作

![image-20240303150810613](../../../../StudyNote/JavaWeb/assets/image-20240303150810613.png)

注意web.xml的路径，中间的web非常容易掉！

![image-20240303150950791](../../../../StudyNote/JavaWeb/assets/image-20240303150950791.png)

然后点击确定，应用即可。

# 