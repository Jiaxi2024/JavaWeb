# JavaWeb

(下载的maven和项目所有的资料都是基于jdk11版本）
缺：day123，day4maven

## day4  

### Maven  

#### Maven的作用

* 方便快捷的管理项目的依赖jar包，避免版本冲突
* 提供标准统一的项目结构（无论用哪一款开发工具eclipse/IDEA/...，都可以互相导入了）
* 标准跨平台（Linux,Windows,MacOS)的自动化项目构建方式

#### 仓库 存储管理各种jar包
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


### Web Spring

#### SpringBootWeb入门
**报错**
* 运行springboot demo的时候报错：Cannot resolve symbol Springframework
  * IDEA右侧maven点击reload all maven projects
  * 或者：https://stackoverflow.com/questions/49364261/cannot-resolve-symbol-springapplication/49364667

* 不要忘记添加请求处理方法，并添加注释
  * 比如    @RequestMapping("/hello")    打开http://localhost:8080/hello  这里的http://localhost:8080后的/hello就是指定的请求路径


#### HTTP协议

1. 基于TCP协议：面向连接、安全
2. 一次请求对应一次相应
3. HTTP协议是无状态协议：对于事务处理没有记忆能力（多次请求中不能共享数据）

*
#### Web服务器-Tomcat


