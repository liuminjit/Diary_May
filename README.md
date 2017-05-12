# Diary_May
## Table of Contents
  1. [2017-5-2 用 Inno Setup Compiler 打包笔记](#diary--201752--)
  
  2. [对 HTTP 304 的理解](#diary--2017512-)
## Diary 【 2017/5/2 】   
### 用 Inno Setup Compiler 打包笔记 2017/5/2 
    Create a new script file using the Script Wizard  
    用script wizard(脚本向导)脚本向导创建一个新的脚本文件
#### 第一页面
    * Application name : 应用程序名称
    * Application version :  应用程序版本
    Application publisher : 应用程序发行商
    Application website : 应用程序网站 (安装后期若选择在WEB中访问的地址)
	    next >
#### 第二页面
    * Application folder name : 应用程序文件夹名称(磁盘文件夹)
    ( 安装文件夹的名称 ———— C:\Program Files (x86)\应用程序文件夹名称——zcc磁盘文件夹)
	    next >
#### 第三页面
    * Application main executable file : 应用程序主要执行文件 (exe结尾)
    √ Allow user to start the application after Setup has finished (允许用户安装完启动)
    * Add file(s) Add folder
 	    next >
####  第四页面
    * Create a shortcut to the main executable in the common Start Menu Programs folder 
    or Application Start Menu folder name (创建开始菜单中文件夹名 )
 	    next >
#### 第五页面
    License file ( 许可证 )
    Information file shown before installation (txt 安装前显示的信息)
    Information file shown after installation (txt 安装后显示的信息)
 	  next >
#### 第六页面
  	选择语言
  	next >
#### 第七页面
	* Custom compiler output folder ( 自定义编译器输出的文件夹)
	* Compiler output base file name  ( 编译输出基础文件名 ——安装包名 )
	* Custom Setup icon file ( 自定义设置图标文件 icon)
	* Setup password ( 安装密码 )
	next >
#### 第八个页面
    * use #define compiler directives ( 使用#定义编译器指令 )
  	     Finish 
## Diary 【 2017/5/12 】	  
### 对 HTTP 304 的理解 2017/5/12 	

    Web的Cache问题:
    304 的标准解释是：Not Modified 客户端有缓冲的文档并发出了一个条件性的请求
    （一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）
    服务器告诉客户，原来缓冲的文档还可以继续使用
    如果客户端在请求一个文件的时候，发现自己缓存的文件有 Last Modified ，那么在请求中会包含 If Modified Since ，这个时间就是缓存文件的 Last Modified 因此，如果请求中包含 If Modified Since，就说明已经有缓存在客户端
    只要判断这个时间和当前请求的文件的修改时间就可以确定是返回 304 还是 200 。对于静态文件，例如：CSS、图片，服务器会自动完成 Last Modified 和 If Modified Since 的比较，完成缓存或者更新。但是对于动态页面，就是动态产生的页面，往往没有包含 Last Modified 信息，这样浏览器、网关等都不会做缓存，也就是在每次请求的时候都完成一个 200 的请求

    因此，对于动态页面做缓存加速，首先要在 Response 的 HTTP Header 中增加 Last Modified 定义，其次根据 Request 中的 If Modified Since 和被请求内容的更新时间来返回 200 或者 304 。虽然在返回 304 的时候已经做了一次数据库查询，但是可以避免接下来更多的数据库查询，并且没有返回页面内容而只是一个 HTTP Header，从而大大的降低带宽的消耗，对于用户的感觉也是提高。
    当这些缓存有效的时候，通过 HttpWatch 查看一个请求会得到这样的结果：
    第一次访问 200
    鼠标点击二次访问 (Cache)
    按F5刷新 304
    按Ctrl+F5强制刷新 200
    如果是这样的就说明缓存真正有效了。以上就是我对 HTTP 304 的一个理解。
