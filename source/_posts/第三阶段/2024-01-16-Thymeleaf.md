---
title: Thymeleaf
tags:
  - JavaWeb
  - Thymeleaf
categories:
  - JavaWeb
copyright_author: 尚硅谷
copyright_author_href: 'https://atguigu.com'
copyright_url: 'https://atguigu.com'
copyright_info: 此文章版权归尚硅谷所有，如有转载，请注明来自原作者
abbrlink: b12a1160
date: 2024-01-16 23:30:17
description:

---

### Thmeleaf

### 1. 内容讲解

#### 1. MVC

##### 1. 为什么需要MVC

我们如果只使用HTML作为视图的话，它是无法展示动态数据的，所以我们对HTML的新的期待：既能够正常显示页面，又能在页面中包含动态数据部分。而动态数据单靠HTML本身是无法做到的，所以此时我们需要引入服务器端动态视图模板技术。

##### 2 .MVC概念

M：Model模型

V：View视图

C：Controller控制器

MVC是在表述层开发中运用的一种设计理念。主张把**封装数据的『模型』**、**显示用户界面的『视图』**、**协调调度的『控制器』**分开。

好处：

- 进一步实现各个组件之间的解耦
- 让各个组件可以单独维护
- 将视图分离出来以后，我们后端工程师和前端工程师的对接更方便

##### 3. MVC和三层架构之间关系

![images](https://blog-resources.this0.com/image/202403301647734.png?x-oss-process=style/this0-blog )

#### 2 .Thymeleaf的简介

##### 1. Thymeleaf的概念

Thymeleaf是一款用于渲染XML/XHTML/HTML5内容的模板引擎。类似JSP，Velocity，FreeMaker等， 它也可以轻易的与Spring MVC等Web框架进行集成作为Web应用的模板引擎。它的主要作用是在静态页面上渲染显示动态数据

##### 2.Thymeleaf的优势

- SpringBoot官方推荐使用的视图模板技术，和SpringBoot完美整合。

- 不经过服务器运算仍然可以直接查看原始值，对前端工程师更友好。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <p th:text="${username}">Original Value</p>

</body>
</html>
```

##### 3. 物理视图和逻辑视图

###### ① 物理视图

在Servlet中，将请求转发到一个HTML页面文件时，使用的完整的转发路径就是<span style="color:blue;font-weight:bold;">物理视图</span>。![images](https://blog-resources.this0.com/image/202403301647174.png?x-oss-process=style/this0-blog )

> /pages/user/login_success.html

如果我们把所有的HTML页面都放在某个统一的目录下，那么转发地址就会呈现出明显的规律：

> /pages/user/login.html
> /pages/user/login_success.html
> /pages/user/regist.html
> /pages/user/regist_success.html
>
> ……

路径的开头都是：/pages/user/

路径的结尾都是：.html

所以，路径开头的部分我们称之为<span style="color:blue;font-weight:bold;">视图前缀</span>，路径结尾的部分我们称之为<span style="color:blue;font-weight:bold;">视图后缀</span>。

###### ② 逻辑视图

### 2. 逻辑视图

物理视图=视图前缀+逻辑视图+视图后缀

上面的例子中：

| 视图前缀     | 逻辑视图      | 视图后缀 | 物理视图                       |
| ------------ | ------------- | -------- | ------------------------------ |
| /pages/user/ | login         | .html    | /pages/user/login.html         |
| /pages/user/ | login_success | .html    | /pages/user/login_success.html |

#### 1. Thymeleaf的HelloWorld

##### 1. 加入jar包

![images](https://blog-resources.this0.com/image/202403301647933.png?x-oss-process=style/this0-blog )

##### 2. 配置上下文参数

![images](https://blog-resources.this0.com/image/202403301647759.png?x-oss-process=style/this0-blog)

物理视图=视图前缀+逻辑视图+视图后缀

```xml
<!-- 在上下文参数中配置视图前缀和视图后缀 -->
<context-param>
    <param-name>view-prefix</param-name>
    <param-value>/WEB-INF/view/</param-value>
</context-param>
<context-param>
    <param-name>view-suffix</param-name>
    <param-value>.html</param-value>
</context-param>
```

说明：param-value中设置的前缀、后缀的值不是必须叫这个名字，可以根据实际情况和需求进行修改。

> 为什么要放在WEB-INF目录下？
>
> 原因：WEB-INF目录不允许浏览器直接访问，所以我们的视图模板文件放在这个目录下，是一种保护。以免外界可以随意访问视图模板文件。
>
> 访问WEB-INF目录下的页面，都必须通过Servlet转发过来，简单说就是：不经过Servlet访问不了。
>
> 这样就方便我们在Servlet中检查当前用户是否有权限访问。
>
> 那放在WEB-INF目录下之后，重定向进不去怎么办？
>
> 重定向到Servlet，再通过Servlet转发到WEB-INF下。

##### 3. 创建Servlet基类

这个类大家直接<span style="color:blue;font-weight:bold;">复制粘贴</span>即可，将来使用框架后，这些代码都将被取代。

```java
import org.thymeleaf.TemplateEngine;
import org.thymeleaf.context.WebContext;
import org.thymeleaf.templatemode.TemplateMode;
import org.thymeleaf.templateresolver.ServletContextTemplateResolver;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ViewBaseServlet extends HttpServlet {

    private TemplateEngine templateEngine;

    @Override
    public void init() throws ServletException {

        // 1.获取ServletContext对象
        ServletContext servletContext = this.getServletContext();

        // 2.创建Thymeleaf解析器对象
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(servletContext);

        // 3.给解析器对象设置参数
        // ①HTML是默认模式，明确设置是为了代码更容易理解
        templateResolver.setTemplateMode(TemplateMode.HTML);

        // ②设置前缀
        String viewPrefix = servletContext.getInitParameter("view-prefix");

        templateResolver.setPrefix(viewPrefix);

        // ③设置后缀
        String viewSuffix = servletContext.getInitParameter("view-suffix");

        templateResolver.setSuffix(viewSuffix);

        // ④设置缓存过期时间（毫秒）
        templateResolver.setCacheTTLMs(60000L);

        // ⑤设置是否缓存
        templateResolver.setCacheable(true);

        // ⑥设置服务器端编码方式
        templateResolver.setCharacterEncoding("utf-8");

        // 4.创建模板引擎对象
        templateEngine = new TemplateEngine();

        // 5.给模板引擎对象设置模板解析器
        templateEngine.setTemplateResolver(templateResolver);

    }

    protected void processTemplate(String templateName, HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // 1.设置响应体内容类型和字符集
        resp.setContentType("text/html;charset=UTF-8");

        // 2.创建WebContext对象
        WebContext webContext = new WebContext(req, resp, getServletContext());

        // 3.处理模板数据
        templateEngine.process(templateName, webContext, resp.getWriter());
    }
}
```

##### 4 .入门案例代码

###### ① 创建index.html文件

![images](https://blog-resources.this0.com/image/202403301647933.png?x-oss-process=style/this0-blog)

###### ② index.html编写超链接访问Servlet

```html
<a href="/webday08/TestThymeleafServlet">初步测试Thymeleaf</a>
```

###### ③ 创建Servlet

![images](https://blog-resources.this0.com/image/202403301647730.png?x-oss-process=style/this0-blog)

```xml
<servlet>
    <servlet-name>testThymeleafServlet</servlet-name>
    <servlet-class>com.atguigu.servlet.TestThymeleafServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>testThymeleafServlet</servlet-name>
    <url-pattern>/testThymeleaf</url-pattern>
</servlet-mapping>
```

###### ④ 修改Servlet让其继承ViewBaseServlet

![images](https://blog-resources.this0.com/image/202403301647321.png?x-oss-process=style/this0-blog )

###### ⑤ 在doPost()方法中跳转到Thymeleaf页面

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-06-13  09:15
 */
public class TestThymeleafServlet extends ViewBaseServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("username","奥巴马");
        //请求转发跳转到/WEB-INF/view/target.html
        processTemplate("target",request,response);
    }
}
```

###### ⑥ Thymeleaf页面代码

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>目标页面</title>
    </head>
    <body>
        <h1 th:text="${username}">这里要显示一个动态的username</h1>
    </body>
</html>
```

### 3. Thymeleaf的基本语法

#### 1 .th名称空间

![images](https://blog-resources.this0.com/image/202403301647394.png?x-oss-process=style/this0-blog)

#### 2 .表达式语法

##### 1 .修改标签文本值

代码示例：

```html
<p th:text="标签体新值">标签体原始值</p>
```

###### ① th:text作用

- 不经过服务器解析，直接用浏览器打开HTML文件，看到的是『标签体原始值』
- 经过服务器解析，Thymeleaf引擎根据th:text属性指定的『标签体新值』去<span style="color:blue;font-weight:bold;">替换</span>『标签体原始值』

##### 2 .修改指定属性值

代码示例：

```html
<input type="text" name="username" th:value="文本框新值" value="文本框旧值" />
```

语法：任何HTML标签原有的属性，前面加上『th:』就都可以通过Thymeleaf来设定新值。

##### 3. 解析URL地址

代码示例：

```html
<!--
使用Thymeleaf解析url地址
-->
<a th:href="@{/index.html}">访问index.html</a>
```

经过解析后得到：

> /webday08/index.html

所以@{}的作用是<span style="color:blue;font-weight:bold;">在字符串前附加『上下文路径』</span>

>  这个语法的好处是：实际开发过程中，项目在不同环境部署时，Web应用的名字有可能发生变化。所以上下文路径不能写死。而通过@{}动态获取上下文路径后，不管怎么变都不怕啦！

###### ① 首页使用URL地址解析

![images](https://blog-resources.this0.com/image/202403301647259.png?x-oss-process=style/this0-blog)

如果我们直接访问index.html本身，那么index.html是不需要通过Servlet，当然也不经过模板引擎，所以index.html上的Thymeleaf的任何表达式都不会被解析。

解决办法：通过Servlet访问index.html，这样就可以让模板引擎渲染页面了：

![images](https://blog-resources.this0.com/image/202403301647393.png?x-oss-process=style/this0-blog)

> 进一步的好处：
>
> 通过上面的例子我们看到，所有和业务功能相关的请求都能够确保它们通过Servlet来处理，这样就方便我们统一对这些请求进行特定规则的限定。

###### ② 给URL地址后面附加请求参数

参照官方文档说明

#### 3. 域对象在Thymeleaf中的使用

##### 1. 回顾域对象

域对象是在服务器中有一定作用域范围的对象，在这个范围内的所有动态资源都能够共享域对象中保存的数据

##### 2 .回顾域对象的类型

###### ① 请求域

在请求转发的场景下，我们可以借助HttpServletRequest对象内部给我们提供的存储空间，帮助我们携带数据，把数据发送给转发的目标资源。

请求域：HttpServletRequest对象内部给我们提供的存储空间

###### ② 会话域(后面学)

会话域的范围是一次会话

![images](https://blog-resources.this0.com/image/202403301647460.png?x-oss-process=style/this0-blog)

###### ③ 应用域(后面学)

应用域的范围是整个项目全局

![](https://blog-resources.this0.com/image/202403301647616.png?x-oss-process=style/this0-blog)

##### 3. 在Thymeleaf中操作域对象

我们通常的做法是，在Servlet中将数据存储到域对象中，而在使用了Thymeleaf的前端页面中取出域对象中的数据并展示

###### ① 操作请求域

Servlet中代码：

```java
String requestAttrName = "helloRequestAttr";
String requestAttrValue = "helloRequestAttr-VALUE";

request.setAttribute(requestAttrName, requestAttrValue);
```

Thymeleaf表达式：

```html
<p th:text="${helloRequestAttr}">request field value</p>
```

###### ② 操作会话域(后期讲)

###### ③ 操作应用域(后期讲)

#### 4 .获取请求参数

##### 1 .获取请求参数的语法

```html
${param.参数名}
```

##### 2 .根据一个参数名获取一个参数值

页面代码：

```html
<p th:text="${param.username}">这里替换为请求参数的值</p>
```

页面显示效果：

![images](https://blog-resources.this0.com/image/202403301647616.png?x-oss-process=style/this0-blog)

##### 3 .根据一个参数名获取多个参数值

页面代码：

```html
<p th:text="${param.team}">这里替换为请求参数的值</p>
```

页面显示效果：

![images](https://blog-resources.this0.com/image/202403301647642.png?x-oss-process=style/this0-blog )

如果想要精确获取某一个值，可以使用数组下标。页面代码：

```html
<p th:text="${param.team[0]}">这里替换为请求参数的值</p>
<p th:text="${param.team[1]}">这里替换为请求参数的值</p>
```

页面显示效果：

![images](https://blog-resources.this0.com/image/202403301647730.png?x-oss-process=style/this0-blog )

#### `5 .内置对象`

##### 1 .内置对象的概念

`所谓内置对象其实就是在Thymeleaf的表达式中<span style="color:blue;font-weight:bold;">可以直接使用</span>的对象`

##### 2 .基本内置对象

用法举例：

```html
<h3>表达式的基本内置对象</h3>
<p th:text="${#request.getContextPath()}">调用#request对象的getContextPath()方法</p>
<p th:text="${#request.getAttribute('helloRequestAttr')}">调用#request对象的getAttribute()方法，读取属性域</p>
```

基本思路：

- 如果不清楚这个对象有哪些方法可以使用，那么就通过getClass().getName()获取全类名，再回到Java环境查看这个对象有哪些方法
- `内置对象的方法可以直接调用`
- 调用方法时需要传参的也可以直接传入参数

##### 3. 公共内置对象

Servlet中将List集合数据存入请求域：

```java
request.setAttribute("aNotEmptyList", Arrays.asList("aaa","bbb","ccc"));
request.setAttribute("anEmptyList", new ArrayList<>());
```

页面代码：

```html
<p>#list对象isEmpty方法判断集合整体是否为空aNotEmptyList：<span th:text="${#lists.isEmpty(aNotEmptyList)}">测试#lists</span></p>
<p>#list对象isEmpty方法判断集合整体是否为空anEmptyList：<span th:text="${#lists.isEmpty(anEmptyList)}">测试#lists</span></p>
```

#### 6. OGNL

##### 1 .OGNL的概念

OGNL：Object-Graph Navigation Language对象-图 导航语言

##### 2 .对象图的概念

从根对象触发，通过特定的语法，逐层访问对象的各种属性。

![images](https://blog-resources.this0.com/image/202403301647853.png?x-oss-process=style/this0-blog )

##### 3 .OGNL语法

###### ① 起点

在Thymeleaf环境下，${}中的表达式可以从下列元素开始：

- 访问属性域的起点
  - 请求域属性名
  - session
  - application
- param
- `内置对象`
  - `request`
  - `session`
  - `lists`
  - `strings`

###### ② 属性访问语法

- 访问对象属性：使用getXxx()、setXxx()方法定义的属性
  - 对象.属性名
- 访问List集合或数组
  - 集合或数组[下标]
- 访问Map集合
  - Map集合.key
  - Map集合['key']

#### 7 .分支与迭代

##### 1 .分支

###### ① if和unless

让标记了th:if、th:unless的标签根据条件决定是否显示。

示例的实体类：

```java
package com.atguigu.bean;

public class Teacher {
    private String teacherName;

    public Teacher() {
    }

    public Teacher(String teacherName) {
        this.teacherName = teacherName;
    }

    public String getTeacherName() {
        return teacherName;
    }

    public void setTeacherName(String teacherName) {
        this.teacherName = teacherName;
    }
}
```

示例的Servlet代码：

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.创建ArrayList对象并填充
    List<Employee> employeeList = new ArrayList<>();

    employeeList.add(new Employee(1, "tom", 500.00));
    employeeList.add(new Employee(2, "jerry", 600.00));
    employeeList.add(new Employee(3, "harry", 700.00));

    // 2.将集合数据存入请求域
    request.setAttribute("employeeList", employeeList);

    // 3.调用父类方法渲染视图
    super.processTemplate("list", request, response);
}
```

示例的HTML代码：

```html
<table>
    <tr>
        <th>员工编号</th>
        <th>员工姓名</th>
        <th>员工工资</th>
    </tr>
    <tr th:if="${#lists.isEmpty(employeeList)}">
        <td colspan="3">抱歉！没有查询到你搜索的数据！</td>
    </tr>
    <tr th:if="${not #lists.isEmpty(employeeList)}">
        <td colspan="3">有数据！</td>
    </tr>
    <tr th:unless="${#lists.isEmpty(employeeList)}">
        <td colspan="3">有数据！</td>
    </tr>
</table>
```

if配合not关键词和unless配合原表达式效果是一样的，看自己的喜好。

###### ② switch

```html
<h3>测试switch</h3>
<div th:switch="${user.memberLevel}">
    <p th:case="level-1">银牌会员</p>
    <p th:case="level-2">金牌会员</p>
    <p th:case="level-3">白金会员</p>
    <p th:case="level-4">钻石会员</p>
</div>
```

##### 2 .迭代

`在迭代过程中，可以参考下面的说明使用迭代状态：`

![images](https://blog-resources.this0.com/image/202403301647817.png?x-oss-process=style/this0-blog)

```html
<!--遍历显示请求域中的teacherList-->
<table border="1" cellspacing="0" width="500">
    <tr>
        <th>编号</th>
        <th>姓名</th>
    </tr>
    <tbody th:if="${#lists.isEmpty(teacherList)}">
        <tr>
            <td colspan="2">教师的集合是空的!!!</td>
        </tr>
    </tbody>

    <!--
集合不为空，遍历展示数据
-->
    <tbody th:unless="${#lists.isEmpty(teacherList)}">
        <!--
使用th:each遍历
用法:
1. th:each写在什么标签上？ 每次遍历出来一条数据就要添加一个什么标签，那么th:each就写在这个标签上
2. th:each的语法    th:each="遍历出来的数据,数据的状态 : 要遍历的数据"
3. status表示遍历的状态，它包含如下属性:
 index 遍历出来的每一个元素的下标
 count 遍历出来的每一个元素的计数

-->
        <tr th:each="teacher,status : ${teacherList}">
            <td th:text="${status.count}">这里显示编号</td>
            <td th:text="${teacher.teacherName}">这里显示老师的名字</td>
        </tr>
    </tbody>
</table>
```

#### 8 .Thymeleaf包含其他模板文件

##### 1 .应用场景

抽取各个页面的公共部分：

![images](https://blog-resources.this0.com/image/202403301647081.png?x-oss-process=style/this0-blog )

##### 2 .操作步骤

###### ① 创建页面的公共代码片段

使用th:fragment来给这个片段命名：

```html
<div th:fragment="header">
    <p>被抽取出来的头部内容</p>
</div>
```

###### `② 在需要的页面中进行包含`

| 语法       | 效果                                                     | 特点                                       |
| ---------- | -------------------------------------------------------- | ------------------------------------------ |
| th:insert  | 把目标的代码片段整个插入到当前标签内部                   | 它会保留页面自身的标签                     |
| th:replace | 用目标的代码替换当前标签                                 | 它不会保留页面自身的标签                   |
| th:include | 把目标的代码片段去除最外层标签，然后再插入到当前标签内部 | 它会去掉片段外层标记，同时保留页面自身标记 |

页面代码举例：

```html
<!-- 代码片段所在页面的逻辑视图 :: 代码片段的名称 -->
<div id="badBoy" th:insert="segment :: header">
    div标签的原始内容
</div>

<div id="worseBoy" th:replace="segment :: header">
    div标签的原始内容
</div>

<div id="worstBoy" th:include="segment :: header">
    div标签的原始内容
</div>
```



