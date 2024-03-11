---
title: SpringBoot3
tags:
  - SpringBoot
categories:
  - SpringBoot
copyright_author: 尚硅谷
copyright_author_href: 'https://atguigu.com'
copyright_url: 'https://atguigu.com'
copyright_info: 此文章版权归尚硅谷所有，如有转载，请注明来自原作者
abbrlink: 55656a4d
date: 2024-01-31 00:11:01
description:
---

## 二、SpringBoot3配置文件

## 三、SpringBoot3整合SpringMVC

### 3.1 实现过程

1. 创建程序

2. 引入依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>3.0.5</version>
       </parent>
   
       <groupId>com.atguigu</groupId>
       <artifactId>springboot-starter-springmvc-03</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <properties>
           <maven.compiler.source>17</maven.compiler.source>
           <maven.compiler.target>17</maven.compiler.target>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       </properties>
   
       <dependencies>
           <!--        web开发的场景启动器 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
   
   </project>
   ```

3. 创建启动类

   ```java
   @SpringBootApplication
   public class MainApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(MainApplication.class,args);
       }
   }
   
   ```

4. 创建实体类

   ```java
   package com.atguigu.pojo;
   
   public class User {
       private String username ;
       private String password ;
       private Integer age ;
       private String sex ;
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   
       public String getPassword() {
           return password;
       }
   
       public void setPassword(String password) {
           this.password = password;
       }
   
       public Integer getAge() {
           return age;
       }
   
       public void setAge(Integer age) {
           this.age = age;
       }
   
       public String getSex() {
           return sex;
       }
   
       public void setSex(String sex) {
           this.sex = sex;
       }
   }
   ```

5. 编写Controller

   ```java
   package com.atguigu.controller;
   
   import com.atguigu.pojo.User;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       @GetMapping("/getUser")
       @ResponseBody
       public User getUser(){
           
           User user = new User();
           user.setUsername("杨过");
           user.setPassword("123456");
           user.setAge(18);
           user.setSex("男");
           return user;
       }
   }
   ```

6. 访问测试

   ![](http://cdn.this0.com/blog/img/image_F2x330aDA1.png)

### 3.2 web相关配置

位置：application.yml

```yaml
# web相关的配置
# https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.server
server:
  # 端口号设置
  port: 80
  # 项目根路径
  servlet:
    context-path: /boot
```

当涉及Spring Boot的Web应用程序配置时，以下是五个重要的配置参数：

`//TODO，五个参数，后面看`

1.  `server.port`: 指定应用程序的HTTP服务器端口号。默认情况下，Spring Boot使用8080作为默认端口。您可以通过在配置文件中设置`server.port`来更改端口号。
2.  `server.servlet.context-path`: 设置应用程序的上下文路径。这是应用程序在URL中的基本路径。默认情况下，上下文路径为空。您可以通过在配置文件中设置`server.servlet.context-path`属性来指定自定义的上下文路径。
3.  `spring.mvc.view.prefix`和`spring.mvc.view.suffix`: 这两个属性用于配置视图解析器的前缀和后缀。视图解析器用于解析控制器返回的视图名称，并将其映射到实际的视图页面。`spring.mvc.view.prefix`定义视图的前缀，`spring.mvc.view.suffix`定义视图的后缀。
4.  `spring.resources.static-locations`: 配置静态资源的位置。静态资源可以是CSS、JavaScript、图像等。默认情况下，Spring Boot会将静态资源放在`classpath:/static`目录下。您可以通过在配置文件中设置`spring.resources.static-locations`属性来自定义静态资源的位置。
5.  `spring.http.encoding.charset`和`spring.http.encoding.enabled`: 这两个属性用于配置HTTP请求和响应的字符编码。`spring.http.encoding.charset`定义字符编码的名称（例如UTF-8），`spring.http.encoding.enabled`用于启用或禁用字符编码的自动配置。

这些是在Spring Boot的配置文件中与Web应用程序相关的一些重要配置参数。根据您的需求，您可以在配置文件中设置这些参数来定制和配置您的Web应用程序

### 3.3 静态资源处理

> 在WEB开发中我们需要引入一些静态资源 , 例如 : HTML , CSS , JS , 图片等 , 如果是普通的项目静态资源可以放在项目的webapp目录下。现在使用Spring Boot做开发 , 项目中没有webapp目录 , 我们的项目是一个jar工程，那么就没有webapp，我们的静态资源该放哪里呢？

1. 默认路径	//TODO@resources类中查看

   在springboot中就定义了静态资源的默认查找路径：

   ```java
   package org.springframework.boot.autoconfigure.web;
   //..................
   public static class Resources {
           private static final String[] CLASSPATH_RESOURCE_LOCATIONS = new String[]{"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"};
           private String[] staticLocations;
           private boolean addMappings;
           private boolean customized;
           private final Chain chain;
           private final Cache cache;
   
           public Resources() {
               this.staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
               this.addMappings = true;
               this.customized = false;
               this.chain = new Chain();
               this.cache = new Cache();
           }
   //...........        
   ```

   `**默认的静态资源路径为：**`

   **· classpath:/META-INF/resources/**

   **· classpath:/resources/**

   **· classpath:/static/**

   **· classpath:/public/**

   我们只要静态资源放在这些目录中任何一个，SpringMVC都会帮我们处理。 我们习惯会把静态资源放在classpath:/static/ 目录下。在resources目录下创建index.html文件

   ![](http://cdn.this0.com/blog/img/image_UrWg1Ahzpl.png)

   打开浏览器输入 : [http://localhost:8080/index.html](http://localhost:8080/index.html "http://localhost:8080/index.html")

2. 覆盖路径

   ```yaml
   # web相关的配置
   # https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.server
   server:
     # 端口号设置
     port: 80
     # 项目根路径
     servlet:
       context-path: /boot
   spring:
     web:
       resources:
         # 配置静态资源地址,如果设置,会覆盖默认值
         static-locations: classpath:/webapp
   ```

   ![](http://cdn.this0.com/blog/img/image__VfopbeBWz.png)

   访问地址：[http://localhost/boot/login.html](http://localhost/boot/login.html "http://localhost/boot/login.html")

### 3.4 自定义拦截器(SpringMVC配置)	//TODO第三次出现

1. 拦截器声明

   ```java
   package com.atguigu.interceptor;
   
   import jakarta.servlet.http.HttpServletRequest;
   import jakarta.servlet.http.HttpServletResponse;
   import org.springframework.stereotype.Component;
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   @Component
   public class MyInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("MyInterceptor拦截器的preHandle方法执行....");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("MyInterceptor拦截器的postHandle方法执行....");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("MyInterceptor拦截器的afterCompletion方法执行....");
       }
   }
   ```

2. 拦截器配置

   正常使用配置类，只要保证，**配置类要在启动类的同包或者子包方可生效！**

   ```java
   package com.atguigu.config;
   
   import com.atguigu.interceptor.MyInterceptor;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
   import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
   
   @Configuration
   public class MvcConfig implements WebMvcConfigurer {
   
       @Autowired
       private MyInterceptor myInterceptor ;
   
       /**
        * /**  拦截当前目录及子目录下的所有路径 /user/**   /user/findAll  /user/order/findAll
        * /*   拦截当前目录下的以及子路径   /user/*     /user/findAll
        * @param registry
        */
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(myInterceptor).addPathPatterns("/**");
       }
   }
   ```

3. 拦截器效果测试

   ![](http://cdn.this0.com/blog/img/image_cFnsWp9UmJ.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=PA9Qm/jzgVNXGoOFB14QFmfZpao=&x-oss-process=style/cdn.this0)

## 四、SpringBoot3整合Druid数据源	//TODO没成功，算了

1. 创建程序

2. 引入依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>3.0.5</version>
       </parent>
       <groupId>com.atguigu</groupId>
       <artifactId>springboot-starter-druid-04</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <properties>
           <maven.compiler.source>17</maven.compiler.source>
           <maven.compiler.target>17</maven.compiler.target>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       </properties>
   ```


        <dependencies>
            <!--  web开发的场景启动器 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
    
            <!-- 数据库相关配置启动器 jdbctemplate 事务相关-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-jdbc</artifactId>
            </dependency>
    
            <!-- druid启动器的依赖  -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-3-starter</artifactId>
                <version>1.2.18</version>
            </dependency>
    
            <!-- 驱动类-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
            </dependency>
    
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.28</version>
            </dependency>
    
        </dependencies>
    
        <!--    SpringBoot应用打包插件-->
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>
    
    </project>
    ```

3. 启动类

   ```java
   @SpringBootApplication
   public class MainApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(MainApplication.class,args);
       }
   }
   
   ```

4. 配置文件编写

   > 添加druid连接池的基本配置

   ```yaml
   spring:
     datasource:
       # 连接池类型 
       type: com.alibaba.druid.pool.DruidDataSource
   
       # Druid的其他属性配置 springboot3整合情况下,数据库连接信息必须在Druid属性下!
       druid:
         url: jdbc:mysql://localhost:3306/day01
         username: root
         password: root
         driver-class-name: com.mysql.cj.jdbc.Driver
         # 初始化时建立物理连接的个数
         initial-size: 5
         # 连接池的最小空闲数量
         min-idle: 5
         # 连接池最大连接数量
         max-active: 20
         # 获取连接时最大等待时间，单位毫秒
         max-wait: 60000
         # 申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
         test-while-idle: true
         # 既作为检测的间隔时间又作为testWhileIdel执行的依据
         time-between-eviction-runs-millis: 60000
         # 销毁线程时检测当前连接的最后活动时间和当前时间差大于该值时，关闭当前连接(配置连接在池中的最小生存时间)
         min-evictable-idle-time-millis: 30000
         # 用来检测数据库连接是否有效的sql 必须是一个查询语句(oracle中为 select 1 from dual)
         validation-query: select 1
         # 申请连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
         test-on-borrow: false
         # 归还连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
         test-on-return: false
         # 是否缓存preparedStatement, 也就是PSCache,PSCache对支持游标的数据库性能提升巨大，比如说oracle,在mysql下建议关闭。
         pool-prepared-statements: false
         # 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100
         max-pool-prepared-statement-per-connection-size: -1
         # 合并多个DruidDataSource的监控数据
         use-global-data-source-stat: true
   
   logging:
     level:
       root: debug
   ```

5. 编写Controller

   ```java
   @Slf4j
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       @Autowired
       private JdbcTemplate jdbcTemplate;
   
       @GetMapping("/getUser")
       @ResponseBody
       public User getUser(){
           String sql = "select * from users where id = ? ; ";
           User user = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(User.class), 1);
           log.info("查询的user数据为:{}",user.toString());
           return user;
       }
       
   }
   ```

6. 启动测试

7. 问题解决

   通过源码分析，druid-spring-boot-3-starter目前最新版本是1.2.18，虽然适配了SpringBoot3，但缺少自动装配的配置文件，需要手动在resources目录下创建META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports，文件内容如下!

   ```java
   com.alibaba.druid.spring.boot3.autoconfigure.DruidDataSourceAutoConfigure
   ```

   ![](http://cdn.this0.com/blog/img/image_mGXItOEXGv.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=vjOfYfJJviGx6LeI2lq6oFYxsq4=&x-oss-process=style/cdn.this0)

## 五、SpringBoot3整合Mybatis

### 5.1 MyBatis整合步骤

1.  导入依赖：在您的Spring Boot项目的构建文件（如pom.xml）中添加MyBatis和数据库驱动的相关依赖。例如，如果使用MySQL数据库，您需要添加MyBatis和MySQL驱动的依赖。
2.  配置数据源：在`application.properties`或`application.yml`中配置数据库连接信息，包括数据库URL、用户名、密码、mybatis的功能配置等。
3.  创建实体类：创建与数据库表对应的实体类。
4.  创建Mapper接口：创建与数据库表交互的Mapper接口。
5.  创建Mapper接口SQL实现： 可以使用mapperxml文件或者注解方式
6.  创建程序启动类
7.  注解扫描：在Spring Boot的主应用类上添加`@MapperScan`注解，用于扫描和注册Mapper接口。
8.  使用Mapper接口：在需要使用数据库操作的地方，通过依赖注入或直接实例化Mapper接口，并调用其中的方法进行数据库操作。

### 5.2 Mybatis整合实践

1. 创建项目

2. 导入依赖

   ```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>3.0.5</version>
   </parent>
   
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   
       <dependency>
           <groupId>org.mybatis.spring.boot</groupId>
           <artifactId>mybatis-spring-boot-starter</artifactId>
           <version>3.0.1</version>
       </dependency>
   
       <!-- 数据库相关配置启动器 -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-jdbc</artifactId>
       </dependency>
   
       <!-- druid启动器的依赖  -->
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid-spring-boot-3-starter</artifactId>
           <version>1.2.18</version>
       </dependency>
   
       <!-- 驱动类-->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.28</version>
       </dependency>
   
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <version>1.18.28</version>
       </dependency>
   
   </dependencies>
   ```

3. 配置文件

   ```yaml
   server:
     port: 80
     servlet:
       context-path: /
   spring:
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       druid:
         url: jdbc:mysql:///day01
         username: root
         password: root
         driver-class-name: com.mysql.cj.jdbc.Driver
         
   mybatis:
     configuration:  # setting配置
       auto-mapping-behavior: full
       map-underscore-to-camel-case: true
       log-impl: org.apache.ibatis.logging.slf4j.Slf4jImpl
     type-aliases-package: com.atguigu.pojo # 配置别名
     mapper-locations: classpath:/mapper/*.xml # mapperxml位置
   ```

4. 实体类准备

   ```java
   package com.atguigu.pojo;
   
   public class User {
       private String account ;
       private String password ;
       private Integer id ;
   
       public String getAccount() {
           return account;
       }
   
       public void setAccount(String account) {
           this.account = account;
       }
   
       public String getPassword() {
           return password;
       }
   
       public void setPassword(String password) {
           this.password = password;
       }
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       @Override
       public String toString() {
           return "User{" +
                   "account='" + account + '\'' +
                   ", password='" + password + '\'' +
                   ", id=" + id +
                   '}';
       }
   }
   ```

5. Mapper接口准备

   ```java
   public interface UserMapper {
   
       List<User> queryAll();
   }
   ```

6. Mapper接口实现（XML）

   位置：resources/mapper/UserMapper.xml

   ```java
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!-- namespace = 接口的全限定符 -->
   <mapper namespace="com.atguigu.mapper.UserMapper">
   
       <select id="queryAll" resultType="user">
           select * from users
       </select>
   
   </mapper>
   ```

7. 编写三层架构代码

   > 伪代码，不添加业务接口！
   >
   > 1.  controller
   >
   >   ```java
   > @Slf4j
   > @Controller
   > @RequestMapping("/user")
   > public class UserController {
   >   ```

           @Autowired
           private UserService userService;
       
           @GetMapping("/list")
           @ResponseBody
           public List<User> getUser(){
               List<User> userList = userService.findList();
               log.info("查询的user数据为:{}",userList);
               return userList;
           }
       
       }
       ```

   2. service

      ```java
      @Slf4j
      @Service
      public class UserService {
      
          @Autowired
          private UserMapper userMapper;
      
          public List<User> findList(){
              List<User> users = userMapper.queryAll();
              log.info("查询全部数据:{}",users);
              return users;
          }
      }
      ```

8. 启动类和接口扫描

   ```java
   @MapperScan("com.atguigu.mapper") //mapper接口扫描配置
   @SpringBootApplication
   public class MainApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(MainApplication.class,args);
       }
   }
   
   ```

9. 启动测试

### 5.3 声明式事务整合配置	//TODO，第n次出现

依赖导入:

```xml
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

注：SpringBoot项目会自动配置一个 DataSourceTransactionManager，所以我们只需在方法（或者类）加上 @Transactional 注解，就自动纳入 Spring 的事务管理了

```java
@Transactional
public void update(){
    User user = new User();
    user.setId(1);
    user.setPassword("test2");
    user.setAccount("test2");
    userMapper.update(user);
}
```

### 5.4 AOP整合配置

依赖导入:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

直接使用aop注解即可:&#x20;

```java
@Component
@Aspect
public class LogAdvice {

    @Before("execution(* com..service.*.*(..))")
    public void before(JoinPoint joinPoint){
        System.out.println("LogAdvice.before");
        System.out.println("joinPoint = " + joinPoint);
    }

}
```

## 六、SpringBoot3项目打包和运行

### 6.1 添加打包插件

> 在Spring Boot项目中添加`spring-boot-maven-plugin`插件是为了支持将项目打包成可执行的可运行jar包。如果不添加`spring-boot-maven-plugin`插件配置，使用常规的`java -jar`命令来运行打包后的Spring Boot项目是无法找到应用程序的入口点，因此导致无法运行。

```xml
<!--    SpringBoot应用打包插件-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 6.2 执行打包

在idea点击package进行打包

可以在编译的target文件中查看jar包

![](http://cdn.this0.com/blog/img/image_xY_DhdZdAA.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=Zo2cBYWy1qzkyg3/aZUYL8MUSmI=&x-oss-process=style/cdn.this0)

### 6.3 `命令启动和参数说明//TODO`

`java -jar`命令用于在Java环境中执行可执行的JAR文件。下面是关于`java -jar`命令的说明：

```xml
命令格式：java -jar  [选项] [参数] <jar文件名>
```

1.  `-D<name>=<value>`：设置系统属性，可以通过`System.getProperty()`方法在应用程序中获取该属性值。例如：`java -jar -Dserver.port=8080 myapp.jar`。
2.  `-X`：设置JVM参数，例如内存大小、垃圾回收策略等。常用的选项包括：
    -   `-Xmx<size>`：设置JVM的最大堆内存大小，例如 `-Xmx512m` 表示设置最大堆内存为512MB。
    -   `-Xms<size>`：设置JVM的初始堆内存大小，例如 `-Xms256m` 表示设置初始堆内存为256MB。
3.  `-Dspring.profiles.active=<profile>`：指定Spring Boot的激活配置文件，可以通过`application-<profile>.properties`或`application-<profile>.yml`文件来加载相应的配置。例如：`java -jar -Dspring.profiles.active=dev myapp.jar`。

启动和测试：

![](http://cdn.this0.com/blog/img/image_sH3S3fZ9Cy.png)

`注意： -D 参数必须要在jar之前！否者不生效！`
