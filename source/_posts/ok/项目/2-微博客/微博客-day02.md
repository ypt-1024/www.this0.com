## 5 后端接口-文章管理

### 5.1 文章列表

#### （1）ArticleController

```java
package com.atguigu.blog.controller;

import com.atguigu.blog.entity.Article;
import com.atguigu.blog.service.ArticleService;
import com.atguigu.blog.utils.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.Map;

@RestController
@RequestMapping("/article")
public class ArticleController {

    @Autowired
    private ArticleService articleService;

    //文章列表
    @PostMapping("queryList")
    public Result articleList(@RequestBody(required = false) Article article) {
        List<Article> list  = articleService.list(article);
        return Result.ok(list);
    }
}
```

#### （2）ArticleService

```java
package com.atguigu.blog.service;

import com.atguigu.blog.entity.Article;

import java.util.Map;

public interface ArticleService {
    //文章列表
    List<Article> list(Article article);
}
```

#### （3）ArticleServiceImpl

```java
package com.atguigu.blog.service.impl;

import com.atguigu.blog.entity.Article;
import com.atguigu.blog.mapper.ArticleMapper;
import com.atguigu.blog.service.ArticleService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Service
public class ArticleServiceImpl implements ArticleService {

    @Autowired
    private ArticleMapper articleMapper;

    //文章列表
    @Override
    public List<Article> list(Article article) {
        List<Article> list = articleMapper.selectList(article);
        return list;
    }
}
```

#### （4）ArticleMapper

```java
package com.atguigu.blog.mapper;

import com.atguigu.blog.entity.Article;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
@Mapper
public interface ArticleMapper {

    //查询文章列表集合
    List<Article> selectList(@Param("vo") Article article);
}
```

#### （5）ArticleMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.blog.mapper.ArticleMapper">

    <!--查询数据list集合-->
    <select id="selectList" resultType="com.atguigu.blog.entity.Article">
        select * from article
        <where>
            <if test="vo.cid != null and vo.cid !=0">
                cid=#{vo.cid}
            </if>
        </where>
    </select>
</mapper>
```



### 5.2 文章添加

#### （1）添加Controller方法

```java
//添加文章
@PostMapping("addArticle")
public Result addArticle(@RequestBody Article article) {
    articleService.insert(article);
    return Result.ok();
}
```

#### （2）添加Service方法

```java
//添加文章
void insert(Article article);
```

#### （3）添加Service实现方法

```java
//添加文章
@Override
public void insert(Article article) {
    articleMapper.insertArticle(article);
}
```

#### （4）添加Mapper方法

```java
//添加文章
void insertArticle(Article article);
```

#### （4）编写xml文件

```xml
<insert id="insertArticle">
	insert into article(title,content,cid,uid) values(#{title},#{content},#{cid},#{uid})
</insert>
```



### 5.3 文章修改

#### （1）添加Controller方法

```java
//根据id查询
@GetMapping("getArticle/{id}")
public Result getArticle(@PathVariable Long id) {
    Article article = articleService.get(id);
    return Result.ok(article);
}

//修改文章
@PutMapping("updateArticle")
public Result updateArticle(@RequestBody Article article) {
    articleService.update(article);
    return Result.ok();
}
```

#### （2）添加Service方法

```java
//根据id查询
Article get(Long id);

//修改文章
void update(Article article);
```

#### （3）添加Service实现方法

```java
//根据id查询
@Override
public Article get(Long id) {
    return articleMapper.getArticle(id);
}

//修改文章
@Override
public void update(Article article) {
    articleMapper.updateArticle(article);
}
```

#### （4）添加Mapper方法

```java
//根据id查询
Article getArticle(Long id);

//修改文章
void updateArticle(Article article);
```

#### （4）编写xml文件

```xml
<!--根据id查询-->
<select id="getArticle" resultType="com.atguigu.blog.entity.Article">
    select * from article where id=#{id}
</select>

<!--//修改文章-->
<update id="updateArticle" parameterType="com.atguigu.blog.entity.Article">
    update article set title=#{title},content=#{content},cid=#{cid} where id=#{id}
</update>
```



### 5.4 文章删除

#### （1）添加Controller方法

```java
//删除文章
@DeleteMapping("deleteArticle/{id}")
public Result deleteArticle(@PathVariable Long id) {
    articleService.delete(id);
    return Result.ok();
}
```

#### （2）添加Service方法

```java
//删除文章
void delete(Long id);
```

#### （3）添加Service实现方法

```java
//删除文章
@Override
public void delete(Long id) {
    articleMapper.deleteArticle(id);
}
```

#### （4）添加Mapper方法

```java
//删除文章
void deleteArticle(Long id);
```

#### （4）编写xml文件

```xml
<!--删除文章-->
<delete id="deleteArticle">
    delete from article where id=#{id}
</delete>
```



### 5.5 整合Swagger

#### 5.5.1 Swagger

Swagger是一种基于OpenAPI规范的API文档生成工具，它可以根据Java代码中的注解自动生成API接口文档，并提供UI界面进行在线测试和调试。

Swagger为开发人员提供了更加方便、直观的API管理方式，有助于提升API的可读性和可维护性。

Swagger的主要特点包括：

1、自动生成API文档：通过在Java代码中添加Swagger注解，Swagger能够自动地解析API接口的参数、响应等信息，并生成相应的API文档。

2、在线测试接口：Swagger提供了UI界面，可以方便地进行API接口的测试和调试，无需单独使用HTTP客户端来测试接口。

3、支持多种语言和框架：Swagger不仅支持Java语言和Spring框架，还支持多种其他语言和框架，如PHP、Python、Go等。

4、扩展性强：Swagger提供了多种扩展机制和插件，可以满足各种项目的需要，如集成OAuth2、自定义UI等。

Swagger提供的UI界面相比于另外一款Api文档生成工具**Knife4j**较为简陋。

#### 5.5.2 Knife4j

官方文档：https://doc.xiaominfo.com/

Knife4j是一种基于Swagger构建的增强工具，它在Swagger的基础上增加了更多的功能和扩展，提供了更加丰富的API文档管理功能。相比于原版

Swagger，Knife4j的主要特点包括：

1、更加美观的UI界面：Knife4j通过对Swagger UI的修改和优化，实现了更加美观、易用的UI界面，提升了开发人员的体验感。

2、支持多种注解配置方式：除了支持原版Swagger的注解配置方式外，Knife4j还提供了其他几种注解配置方式，如@ApiIgnore、

@ApiOperationSupport等，方便开发人员进行不同场景

下的配置。

3、提供多种插件扩展：Knife4j提供了多种插件扩展，如knife4j-auth、knife4j-rate-limiter等，可以满足不同项目的需求。

4、集成Spring Boot Starter：Knife4j发布了spring-boot-starter-knife4j，可以实现更加便捷的集成，并支持配置文件中的动态属性调整。

官方文档使用地址：https://doc.xiaominfo.com/docs/quick-start

##### （1）引入依赖

在项目的pom.xml文件中加入如下依赖：

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>
    <version>4.1.0</version>
</dependency>
```

##### （2）添加knife4j的配置类

```java
package com.atguigu.blog.config;

import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Contact;
import io.swagger.v3.oas.models.info.Info;
import org.springdoc.core.models.GroupedOpenApi;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Knife4jConfig {

    @Bean
    public GroupedOpenApi adminApi() {      // 创建了一个api接口的分组
        return GroupedOpenApi.builder()
                .group("admin-api")         // 分组名称
                .pathsToMatch("/**")  // 接口请求路径规则
                .build();
    }

    /***
     * @description 自定义接口信息
     */
    @Bean
    public OpenAPI customOpenAPI() {

        return new OpenAPI()
                .info(new Info()
                        .title("微博客API接口文档")
                        .version("1.0")
                        .description("微博客API接口文档")
                        .contact(new Contact().name("atguigu"))); // 设定作者
    }

}
```

##### （3）接口测试

启动项目就可以访问到knife4j所生成的接口文档了。访问地址：http://localhost:8001/doc.html

![image-20230711192210508](img\image-20230711192210508.png)

##### （4）常见注解

在Knife4j中也提供了一些注解，让我们对接口加以说明，常见的注解如下所示：

```shell
@Tag： 用在controller类上，对controller进行说明
@Operation: 用在controller接口方法上对接口进行描述
@Parameters：用在controller接口方法上对单个参数进行描述
@Schema： 用在实体类和实体类属性上，对实体类以及实体类属性进行描述
```

举例说明：

```java
@Tag(name = "首页接口")
public class IndexController {


    @Operation(summary = "用户登录")
    public Result<LoginVo> login(@RequestBody LoginDto loginDto) {
        ...
    }

    @Operation(summary = "用户退出")
    @Parameters(value = {
            @Parameter(name = "令牌参数" , required = true)
    })
    @GetMapping(value = "/logout")
    public Result logout(@RequestHeader(value = "token") String token) {
        ...
    }

}

@Data
@Schema(description = "用户登录请求参数")
public class Login {

    @Schema(description = "用户名")
    private String userName ;

    @Schema(description = "密码")
    private String password ;

}
```



## 6 跨域请求

### 6.1 跨域请求简介

跨域请求：通过一个域的JavaScript脚本和另外一个域的内容进行交互

域的信息：协议、域名、端口号

![image-20230507150620790](img\image-20230507150620790.png) 

同域：当两个域的协议、域名、端口号均相同

如下所示：

![image-20230507150506550](img\image-20230507150506550.png) 

**同源【域】策略**：在浏览器中存在一种安全策略就是同源策略，同源策略（Sameoriginpolicy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。同源策略会阻止一个域的javascript脚本和另外一个域的内容进行交互。



### 6.2 解决跨域

后端服务器开启跨域支持：

在Controller上添加**@CrossOrigin**注解

```java
@RestController
@RequestMapping(value = "/XXXX")
@CrossOrigin
public class XXXXController {

}
```



## 7 后端接口-分类管理

### 7.1 编写Controller

```java
package com.atguigu.blog.controller;

import com.atguigu.blog.entity.Category;
import com.atguigu.blog.service.CategoryService;
import com.atguigu.blog.utils.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/category")
@CrossOrigin
public class CategoryController {

    @Autowired
    private CategoryService categoryService;

    //分类列表
    @PostMapping("queryList")
    public Result articleList() {
        List<Category> list = categoryService.list();
        return Result.ok(list);
    }

    //条件分页查询
    @PostMapping("queryListPage/{current}/{limit}")
    public Result queryListPage(@PathVariable Long current,
                                @PathVariable Long limit,
                                @RequestBody(required = false) Category category) {
        Map<String,Object> data = categoryService.queryPage(current,limit,category);
        return Result.ok(data);
    }

    //添加分类
    @PostMapping("addCategory")
    public Result addCategory(@RequestBody Category category) {
        categoryService.insert(category);
        return Result.ok();
    }

    //根据id查询
    @GetMapping("getCategory/{id}")
    public Result getCategory(@PathVariable Long id) {
        Category category = categoryService.get(id);
        return Result.ok(category);
    }
    //修改分类
    @PutMapping("updateCategory")
    public Result updateCategory(@RequestBody Category category) {
        categoryService.update(category);
        return Result.ok();
    }

    //删除分类
    @DeleteMapping("deleteCategory/{id}")
    public Result deleteCategory(@PathVariable Long id) {
        categoryService.delete(id);
        return Result.ok();
    }
}
```



### 7.2 编写Service

```java
package com.atguigu.blog.service;

import com.atguigu.blog.entity.Category;

import java.util.List;
import java.util.Map;

public interface CategoryService {

    //分类列表
    List<Category> list();

    //添加分类
    void insert(Category category);

    //根据id查询
    Category get(Long id);

    //修改分类
    void update(Category category);

    //删除分类
    void delete(Long id);

    //条件分页查询
    Map<String, Object> queryPage(Long current, Long limit, Category category);
}
```



### 7.3 编写Service实现

```java
package com.atguigu.blog.service.impl;

import com.atguigu.blog.entity.Article;
import com.atguigu.blog.entity.Category;
import com.atguigu.blog.mapper.ArticleMapper;
import com.atguigu.blog.mapper.CategoryMapper;
import com.atguigu.blog.service.CategoryService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Service
public class CategoryServiceImpl implements CategoryService {

    @Autowired
    private CategoryMapper categoryMapper;

    //分类列表
    @Override
    public List<Category> list() {
        List<Category> list = categoryMapper.selectList();
        return list;
    }

    //添加分类
    @Override
    public void insert(Category category) {
        categoryMapper.insertCategory(category);
    }

    //根据id查询
    @Override
    public Category get(Long id) {
        return categoryMapper.getCategory(id);
    }

    //修改分类
    @Override
    public void update(Category category) {
        categoryMapper.updateCategory(category);
    }

    //删除分类
    @Override
    public void delete(Long id) {
        categoryMapper.deleteCategory(id);
    }

    //条件分页查询
    @Override
    public Map<String, Object> queryPage(Long current, Long limit, Category category) {
        //查询总记录数
        Long total = categoryMapper.selectCount(category);

        //分页条件查询
        Long begin = (current-1)*limit;
        List<Category> list = categoryMapper.pageList(begin,limit,category);

        //封装
        Map<String, Object> map = new HashMap<>();
        map.put("total",total);
        map.put("list",list);
        return map;
    }
}
```



### 7.4 编写Mapper

```java
package com.atguigu.blog.mapper;

import com.atguigu.blog.entity.Article;
import com.atguigu.blog.entity.Category;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
@Mapper
public interface CategoryMapper {

    //查询数据list集合
    List<Category> selectList();

    //添加分类
    void insertCategory(Category category);

    //根据id查询
    Category getCategory(Long cid);

    //修改分类
    void updateCategory(Category category);

    //删除分类
    void deleteCategory(Long cid);

    //总记录数
    Long selectCount(Category category);

    //条件分页查询
    List<Category> pageList(@Param("begin") Long begin,
                            @Param("limit") Long limit,
                            @Param("vo") Category category);
}
```



### 7.5 编写Mapper映射文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.blog.mapper.CategoryMapper">

    <!--查询数据list集合-->
    <select id="selectList" resultType="com.atguigu.blog.entity.Category">
        select * from category
    </select>

    <!--查询总记录数-->
    <select id="selectCount" resultType="Long">
        select count(*) from category
        <where>
            <if test="cname!=null and cname!=''">
                cname = #{cname}
            </if>
        </where>
    </select>

    <!--条件分页查询-->
    <select id="pageList" resultType="com.atguigu.blog.entity.Category">
        select * from category
        <where>
            <if test="vo.cname!=null and vo.cname!=''">
                cname = #{vo.cname}
            </if>
        </where>
        limit #{begin},#{limit}
    </select>

    <!--//添加分类-->
    <insert id="insertCategory" parameterType="com.atguigu.blog.entity.Category">
        insert into category(cname) values(#{cname})
    </insert>

    <!--根据id查询-->
    <select id="getCategory" resultType="com.atguigu.blog.entity.Category">
        select * from category where cid=#{cid}
    </select>

    <!--//修改分类-->
    <update id="updateCategory" parameterType="com.atguigu.blog.entity.Category">
        update category set cname=#{cname} where cid=#{cid}
    </update>

    <!--删除分类-->
    <delete id="deleteCategory">
        delete from category where cid=#{cid}
    </delete>
</mapper>
```



## 8 后端接口-用户操作接口

### 8.1 用户登录接口

#### （1）编写UserController

```java
package com.atguigu.blog.controller;

import com.atguigu.blog.entity.User;
import com.atguigu.blog.service.UserService;
import com.atguigu.blog.utils.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    //登录
    @PostMapping("login")
    public Result loginUser(@RequestBody User user) {
        User userExist = userService.login(user);
        if(userExist != null) {
            Map<String,Object> map = new HashMap<>();
            map.put("name",userExist.getUsername());
            map.put("uid",userExist.getUid());
            return Result.ok(map);
        } else {
            return Result.fail();
        }
    }
}
```

#### （2）编写UserService

```java
package com.atguigu.blog.service;

import com.atguigu.blog.entity.User;

public interface UserService {
    //登录
    User login(User user);
}
```

#### （3）编写UserService实现

```java
package com.atguigu.blog.service.impl;

import com.atguigu.blog.entity.User;
import com.atguigu.blog.mapper.UserMapper;
import com.atguigu.blog.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserMapper userMapper;

    //登录
    //根据用户名查询密码，比较输入密码和数据库密码是否一致
    @Override
    public User login(User user) {
        User selectUser = userMapper.loginUser(user.getUsername());
        if(selectUser != null) {
            String inputPass = user.getPassword();
            String databasePass = selectUser.getPassword();
            if(!inputPass.equals(databasePass)) {
                return null;
            }
            return selectUser;
        }
        return null;
    }
}
```

#### （4）编写UserMapper

```java
package com.atguigu.blog.mapper;

import com.atguigu.blog.entity.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

@Repository
@Mapper
public interface UserMapper {

    //登录
    User loginUser(String username);
}
```

#### （5）编写UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.blog.mapper.UserMapper">

    <!--//登录-->
    <select id="loginUser" resultType="com.atguigu.blog.entity.User">
        select * from user where username=#{username}
    </select>
</mapper>
```



### 8.2 用户注册接口

#### （1）编写Controller

```java
//注册
@PostMapping("register")
public Result registerUser(@RequestBody User user) {
    boolean flag = userService.register(user);
    if(flag) {
        return Result.ok();
    } else {
        return Result.fail();
    }
}
```

#### （2）编写Service

```java
//注册
boolean register(User user);
```

#### （3）编写Service实现

```java
//注册
@Override
public boolean register(User user) {
    //根据用户名查询，如果存在相同的，不能注册
    User selectUser = userMapper.loginUser(user.getUsername());
    if(selectUser == null) {
        //注册
        userMapper.insertUser(user);
        return true;
    }
    return false;
}
```

#### （4）编写Mapper

```java
//注册
void insertUser(User user);
```

#### （5）编写Mapper映射文件

```xml
<!--//注册-->
<insert id="insertUser">
    insert into user(username,password,phone,introduction) values(#{username},#{password},#{phone},#{introduction})
</insert>
```



### 8.3 用户修改接口

#### （1）编写Controller

```java
//根据id查询
@GetMapping("getUser/{id}")
public Result getUser(@PathVariable Long id) {
    User user = userService.get(id);
    return Result.ok(user);
}

//修改
@PutMapping("updateUser")
public Result updateUser(@RequestBody User user) {
    userService.update(user);
    return Result.ok();
}
```

#### （2）编写Service

```java
//根据id查询
User get(Long id);

//修改
void update(User user);
```

#### （3）编写Service实现

```java
//根据id查询
@Override
public User get(Long id) {
    return userMapper.getUser(id);
}

//修改
@Override
public void update(User user) {
    userMapper.updateUser(user);
}
```

#### （4）编写Mapper

```java
//根据id查询
User getUser(Long id);

//修改
void updateUser(User user);
```

#### （5）编写Mapper映射文件

```xml
<!--根据id查询-->
<select id="getUser" resultType="com.atguigu.blog.entity.User">
    select * from user where uid=#{id}
</select>

<!--修改-->
<update id="updateUser">
    update user set phone=#{phone},introduction=#{introduction} where uid=#{uid}
</update>
```

