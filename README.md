# JavaWeb

(下载的maven和项目所有的资料都是基于jdk11版本）
缺：day123，day4maven

day4  
## Maven  

### Maven的作用

* 方便快捷的管理项目的依赖jar包，避免版本冲突
* 提供标准统一的项目结构（无论用哪一款开发工具eclipse/IDEA/...，都可以互相导入了）
* 标准跨平台（Linux,Windows,MacOS)的自动化项目构建方式

### 仓库 存储管理各种jar包
1. 本地仓库：自己计算机的一个目录
2. 远程仓库（私服）：一般是公司团队搭建的私有仓库
3. 中央仓库：Maven团队搭建的，全球唯一

>PS: 国内按照maven后,建议你在美国这样做：
还原 settings.xml 中的 mirror 设置：

删除或注释掉 <mirrors> 中的阿里云配置；

Maven 会自动使用官方中央仓库，速度稳定且依赖全。

或者手动配置官方镜像：

xml
Copy code
<mirror>
    <id>central</id>
    <name>Maven Central Mirror</name>
    <url>https://repo.maven.apache.org/maven2/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
保持 localRepository 可以不变


## Web Spring

### SpringBootWeb入门
**报错**
* 运行springboot demo的时候报错：Cannot resolve symbol Springframework
  * IDEA右侧maven点击reload all maven projects
  * 或者：https://stackoverflow.com/questions/49364261/cannot-resolve-symbol-springapplication/49364667

* 不要忘记添加请求处理方法，并添加注释
  * 比如    @RequestMapping("/hello")    打开http://localhost:8080/hello  这里的http://localhost:8080后的/hello就是指定的请求路径


### HTTP协议

* HTTP概述

超文本传输协议，规定了浏览器 和服务器直接的数据传输规则  

1. 基于TCP协议：面向连接、安全
2. 一次请求对应一次相应
3. HTTP协议是无状态协议：对于事务处理没有记忆能力（多次请求中不能共享数据）

* HTTP请求协议
  * 请求行（请求数据第一行）：请求方式、资源路径、协议版本
  * 请求头（第二行开始）：格式key、value
  * 请求体：POST请求
    * 备注：请求方式GET（请求参数在请求行，大小有限制）；请求方式POST（请求参数在请求体，POST请求没有大小限制）

* HTTP相应协议
  * 相应码（响应数据第一行）：协议、状态码、描述
    * 响应码：1xx 响应中 2xx 成功 3xx 重定向 4xx 客户端错误 5xx 服务器错误
  * 响应头：第二行开始，格式key value
  * 响应体：最后一部分，存放响应数据
    
### Web服务器-Tomcat 

Web服务器直接对HTTP协议进行封装，使得开发者不必直接对协议进行操作。主要功能是“提供网上信息的浏览”

 * 启动  
   双击/bin/startup.bat
   
 * 端口
   找到apache-tomcat软件包中的conf/server.xml的以下代码，改port号就可以了（比如浏览器localhost:9000)
    <Connector port="9000" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
   
 * 如何部署到tomcat
   将项目放置到webapps目录下，就部署好了
  
## 请求  

1. Postman    
一款功能强大的网页调试与发送网页HTTP请求的chrome插件（常用于接口调试）

3. 简单参数
4. 实体参数
5. 数组集合参数
6. 日期参数
7. Json参数
8. 路径参数
 
## 响应

## 分层解耦

## MySQL

## MyBatis

## Spring  



