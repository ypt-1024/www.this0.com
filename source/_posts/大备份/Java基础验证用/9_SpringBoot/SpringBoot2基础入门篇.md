SpringBoot2

## 基础入门篇

### 一	开发环境

**➜**  **~** mvn -v 
**Apache Maven 3.8.1 (05c21c65bdfed0f71a2f2ada8b84da59348c4c5d)** 
Maven home: /home/yupengtao/SDE/maven/apache-maven-3.8.1 
Java version: 1.8.0_301, vendor: Oracle Corporation, runtime: /home/yupengtao/SDE/jdk1.8.0_301/jre 
Default locale: zh_CN, platform encoding: UTF-8 
OS name: "linux", version: "5.13.9-arch1-1", arch: "amd64", family: "unix"



### 一	国际通用入门案例：HelloWorld！

1	首先，在pom文件中添加项目父工程（第一次报红就重启一下）

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.3</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

2	添加依赖

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
```

3	编辑主程序类

```java
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        //主类固定写法
        SpringApplication.run(MainApplication.class,args);
    }
}
```

4	测试页面访问功能（不用tomcat了）

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String handle01(){
        return "Hello,Spring Boot 2!";

    }
}
```

5	简化配置

​	配置都在application.properties中

6	fat jar插件部署测试

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 二	了解SpringBoot原理

1	继承父项目的主要作用：依赖管理，几乎声明了所有开发中常用的依赖的版本号，一般可以不写版本号

​	更改依赖版本，在pom.xml文件中进行，如：

```xml
<properties>
    <mysql.version>5.1.43</mysql.version>
</properties>
```

2	场景启动器

```xml
spring-boot-starter-*	官方的
*-spring-boot-starter	第三方

```

### 三	自动配置

1	自动配好tomcat

​		引入tomcat依赖

​		配置Tomcat

2	自动配好SpringMVC

3	自动配好Web常见功能

4	默认的包结构

​	主程序所在包及其下面的所有子包里面的组件都会被默认	

5	各种配置拥有默认值

​	默认配置最终都是映射到MultipartProperties

​	配置文件的值最终都会绑定在每个类上，这个类会在容器中创建对象

​	按需加载所有自动配置项

6	按需加载自动配置项



7	组件添加	 

1	@Configuration详解

```java
/**
 * 配置类里面使用@Bean标注在方法上，给容器注册组件，默认也是单实例的
 * proxyBeanMethods = true，解决组件依赖问题
 */
@Configuration(proxyBeanMethods = true)  //告诉springboot这是一个配置类
public class Myconfig {

    @Bean   //给容器中添加组件，以方法名作为组件的id，返回类型就是组件类型
    public User user01(){
        return new User("zhangsan",22);
    }

    @Bean("tom")    //也可以手动给组件改id
    public Pet tomPet(){
        return new Pet("tomcat");
    }
}
```

2	@Import（User.class）

​	给容器自动创建出对应类型的组件

​	默认组件名字就是全类名

3	@ImportResource("classpath:beans.xml")

​	导入Spring配置文件

4	配置绑定

方式1：

​	@Component

​	@ConfigurationProperties(prefix="mycar")

类里面的xxx属性,可以获得配置文件里的mycar.xxx的值

方式2：

​	前提：必须在配置类中写

开启属性配置功能方式1:

​	@Component

​	@ConfigurationProperties(prefix="mycar")

开启属性配置功能方式2(一般用这种):

​	@EnableConfigurationProperties(Car.class)

​	另一个作用：把指定的组件自动注册到容器中

8	自动配置原理入门（源码分析）

​	跳过

### 四	最佳实践

1	引入场景自动配置

​		配置文件中debug=true，开启自动配置报告，看哪些生效，哪些不生效

2	是否需要修改

​	参照文档修改配置项

五	开发小技巧

1	Lombok

​	安装依赖，并在idea中装上插件

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
</dependency>
```

使用1：构建javabean

```java
@ToString
@NoArgsConstructor
//@AllArgsConstructor
@Data
public class User {
    private String name;
    private Integer age;
    private Pet pet;

    public User(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}
```

使用2：日志功能

```java
@Slf4j
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String handle01(){
        
        log.info("请求进来了");
        return "Hello,Spring Boot 2!";

    }

}
```

2	devtools

使用：ctrl+9 重启(感觉没啥用)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

3	Spring Initailizr(Springboot项目初始化向导)
