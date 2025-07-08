# JavaWeb

(下载的maven和项目所有的资料都是基于jdk11版本）
缺：day123，day4maven，day5-12，13
day6.6 idea中见不到创建好的已经存在的table，然后跳着学从day8开始了

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

```
    <Connector port="9000" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

 * 如何部署到tomcat
   将项目放置到webapps目录下，就部署好了
  
## 请求相应  

_查用 GET，改用 POST；简单参数用 Param，复杂结构用 Body_

1. Postman    
一款功能强大的网页调试与发送网页HTTP请求的chrome插件（常用于接口调试）

2. 简单参数  
前端传递了多少个请求参数，就需要在方法中声明多少个形参来接收。参数名与形参变量名相同，定义形参即可接收参数，会自动进行类型转换。  
如果方法形参名称与请求参数名称不匹配（这时终端测试可能返回null但不会报错），可以使用 @RequestParam 完成映射。

```
前端请求：
GET /user?id=1&name=Tom  

后端接收：
@GetMapping("/user")
public String getUser(@RequestParam("id") int id, @RequestParam("name") String name) {
    return "id=" + id + ", name=" + name;
}
```
   
3. 实体参数  
前端传递多个参数的时候，可以将所有的参数都封装到一个实体类中。   
简单实体对象：请求参数名与形参对象属性名相同，定义POJO接受即可。  
如果是复杂对象（比如class中class），只需按结构层次对应起来就可以了

```
后端接收：
public class User {
    private String name;
    private int age;
    // getters/setters
}

@PostMapping("/addUser")
public String addUser(User user) {
    return "name=" + user.getName();
}
```

4. 数组集合参数   
数组：请求参数名与形参中的数组变量名相同，可以直接使用数组封装  
集合：请求参数名与形参中的数组变量名相同，通过 @RequestParam 绑定参数关系

5. 日期参数(@DataTimeFormat)  
形参前添加@DataTimeFormat注解完成日期参数格式转换  

```
前端请求：
GET /time?date=2025-07-05

后端接收：
@GetMapping("/time")
public String time(@RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate date) {
    return date.toString();
}
```

6. Json参数(@RequestBody)  
JSON 参数比表单参数更适合复杂结构（嵌套对象、数组等）。
JSON数据键名与形参对象属性名相同，定义POJO类型形参即可接收参数，需要使用@RequestBody标识。  

嵌套数组
```
前端请求(注意带双引号）：
{
  "name": "Tom",
  "age": 20
}

后端接收：
@PostMapping("/add")
public String add(@RequestBody User user) {
    return "name=" + user.getName();
}
```

嵌套对象
```
请求体（JSON）：
{
  "name": "Tom",
  "age": 10,
  "address": {
    "province": "beijing",
    "city": "beijing"
  }
}

java接收类（POJO）：
public class User {
    private String name;
    private Integer age;
    private Address address;
}
public class Address {
    private String province;
    private String city;
}

控制器：
@PostMapping("/jsonParam")
public String jsonParam(@RequestBody User user) {
    System.out.println(user);
    return "OK";
}
```

7 . 路径参数(@PathVariable)  
 参数写在 URL 路径中，而不是 ?key=value 的形式。
```
前端请求：
GET /user/1001

后端接收（注意这里/user/{id}没有写死，而是动态的）：
@GetMapping("/user/{id}")
public String getById(@PathVariable("id") Long id) {
    return "ID = " + id;
}
```

8. 响应数据（@ResponseBody)  
@RestController = @Controller+@ResponseBody
@ResponseBody:  
位于Controller类/方法  
将方法返回值直接相应，若返回值类型是实体对象/集合，转换为json格式的
统一相应结果：Result(code,msg,data)

10. day5.08案例还没看
 
## 分层解耦
### 三层结构
1. Controller  
控制层  
2. service  
业务逻辑层    
3. dao  
数据访问层  

### 分层解耦   
内聚：软件内各个功能模块内部的功能联系  
耦合：衡量软件中各个层/模块之间的依赖、关联  
软件设计原则：高内聚低耦合  
解除耦合：创建容器 

### IOC&DI
控制反转IOC：Inversion of Control 。对象的创建控制权由程序自身转移到外部（容器）  
依赖注入：Dependency Injection。 容器为应用程序提供运行时，所依赖的资源  
Bean对象：IOC容器中创建、管理的对象  

@Component  控制反转
@Autowired  依赖注入

day06 
## MySQL  
数据库DB：存储和管理数据的仓库  
SQL:Structured Query Language: 操作关系型数据库的编程语言  

1. 登录mySQL
直接cmd+r命令行输入：mysql -uroot -p

2. SQL通用语法  
可以单行/多行书写，以分号结尾
可以使用空格/缩进增强可读性
不区分大小写  
单行注释：--注释内容 或 # 注释内容；  多行注释： /* 注释内容 */  

3. DDL  
数据库操作：
    1. 查询
    2. 使用
    3. 创建
    4. 删除

4. DML
5. DQL


## MyBatis     
看懂day09 mapper接口+XML映射写法  
day10多条件分页查询-菜品管理，列表页逻辑核心    

Mybatis：持久层（dao，就是三层架构访问数据的）开发，用于简化JDBC开发    

### 入门    
1. 使用mybatis查询用户数据    
    1. 创建springboot工程、数据库表user、实体类user
    2. 引入mybatis相关依赖，配置mybatis
    3. 编写SQL语句（注解/XML）
>bug:
>构建项目并运行成功无报错后，并没有如预期一样在控制台输出表
>原因：没有确保这个实体类的字段是和数据库一一对应的，否则 MyBatis 会映射失败（但不报错，只返回空）。
>
2. 



### 基础增删改查    

### Mybatis动态SQL


## Spring    
day12 看懂自动注入，业务类注册逻辑
day13 MVC 看懂rest api如何接受、相应前端数据

day19 redis基础操作（对应苍穹外卖day05）


