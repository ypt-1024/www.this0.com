# 核心功能篇

## 配置文件篇

### 1	文件类型

## 1.1	properties

同以前的properties用法

## 1.2	yaml

### 1.2.1	简介

YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

非常适合用来做以数据为中心的配置文件

### 1.2.2	基本语法

- key: value；kv之间有空格
- 大小写敏感

- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格

- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释

- 字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义

### 1.2.3、数据类型

- 字面量：单个的、不可再分的值。date、boolean、string、number、null

```yaml
k: v
```

- 对象：键值对的集合。map、hash、set、object 

```yaml
行内写法：  k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
```

- 数组：一组按次序排列的值。array、list、queue

```yaml
行内写法：  k: [v1,v2,v3]
#或者
k:
 - v1
 - v2
 - v3
```

### 1.2.4、配置文件绑定

```java
@ConfigurationProperties(prefix = "person")
@Data
@ToString
public class Person {
    private String userName;
    private Boolean boss;
    private Date birth;
    private Integer age;
    private Pet pet;
    private String[] interests;
    private List<String> animal;
    private Map<String, Object> score;
    private Set<Double> salarys;
    private Map<String, List<Pet>> allPets;
}
```

```yaml
# yaml表示以上对象
person:
  userName: zhangsan
  boss: false
  birth: 2019/12/12 20:12:33
  age: 18
  pet:
    name: tomcat
    weight: 23.4
  interests: [篮球,游泳]
  animal:
    - jerry
    - mario
  score:
    english:
      first: 30
      second: 40
      third: 50
    math: [131,140,148]
    chinese: {first: 128,second: 136}
  salarys: [3999,4999.98,5999.99]
  allPets:
    sick:
      - {name: tom}
      - {name: jerry,weight: 47}
    health: [{name: mario,weight: 47}]
```

绑定所需依赖

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

## WEB开发篇

用spring创建向导创建项目，添加相关依赖，创建.yaml文件

### 1	web场景

1	静态资源默认目录

​	四个静态资源路径	META-INF.resources	public	resources	static

​	先当页面访问，没得再找静态资源

2	静态资源前缀

默认无前缀，一般手动配一个前缀

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

3	改变默认的静态资源路径 

```yaml
  web:
    resources:
      static-locations: [classpath:/haha/]
```

4	webjars？

5	欢迎页支持

​	可以配置静态资源路径

​	但是不可以配置静态资源的访问前缀，否则导致index.html失效

6	自定义Favicon

### 2	请求参数处理

#### 2.1	rest使用与原理

#### 2.2	普通参数与基本注解

@PathVariable（路径变量）、@RequestHeader（获取请求头（单个或者所有都可以获取））、@ModelAttribute、@RequestParam（获取请求参数）、@MatrixVariable、@CookieValue(获取cookie值)、@RequestBody（获取请求体，只有post才有）

### 3	拦截器与静态资源放行

#### 3.1	HandlerInterceptor接口

```java
  /**
     * 登录检查
     * 1、配置好拦截器要拦截哪些请求
     * 2、把这些配置放在容器中
     */
    @Slf4j
    public class LoginInterceptor implements HandlerInterceptor {

        /**
         * 目标方法执行之前
         * @param request
         * @param response
         * @param handler
         * @return
         * @throws Exception
         */
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

            String requestURI = request.getRequestURI();
            log.info("preHandle拦截的请求路径是{}",requestURI);

            //登录检查逻辑
            HttpSession session = request.getSession();

            Object loginUser = session.getAttribute("loginUser");

            if(loginUser != null){
                //放行
                return true;
            }

            //拦截住。未登录。跳转到登录页
            request.setAttribute("msg","请先登录");
//        re.sendRedirect("/");
            request.getRequestDispatcher("/").forward(request,response);
            return false;
        }

        /**
         * 目标方法执行完成以后
         * @param request
         * @param response
         * @param handler
         * @param modelAndView
         * @throws Exception
         */
        @Override
        public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
            log.info("postHandle执行{}",modelAndView);
        }

        /**
         * 页面渲染以后
         * @param request
         * @param response
         * @param handler
         * @param ex
         * @throws Exception
         */
        @Override
        public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
            log.info("afterCompletion执行异常{}",ex);
        }
    }
```

#### 3.2	配置拦截哪些请求路径

```java
/**
 * 1、编写一个拦截器实现HandlerInterceptor接口
 * 2、拦截器注册到容器中（实现WebMvcConfigurer的addInterceptors）
 * 3、指定拦截规则【如果是拦截所有，静态资源也会被拦截】
 */
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**")  //所有请求都被拦截包括静态资源
                .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**"); //放行的请求
    }
}
```

#### 3.3	拦截器原理

### 4	视图解析与模版引擎

### 5	文件上传与下载

#### 1	页面

```html
<form method="post" action="/upload" enctype="multipart/form-data">
    <input type="file" name="file"><br>
    <input type="submit" value="提交">
</form>
```

#### 2	java代码

```java
/**
 * MultipartFile 自动封装上传过来的文件
 * @param email
 * @param username
 * @param headerImg
 * @param photos
 * @return
 */
@PostMapping("/upload")
public String upload(@RequestParam("email") String email,
                     @RequestParam("username") String username,
                     @RequestPart("headerImg") MultipartFile headerImg,
                     @RequestPart("photos") MultipartFile[] photos) throws IOException {

    log.info("上传的信息：email={}，username={}，headerImg={}，photos={}",
            email,username,headerImg.getSize(),photos.length);

    if(!headerImg.isEmpty()){
        //保存到文件服务器，OSS服务器
        String originalFilename = headerImg.getOriginalFilename();
        headerImg.transferTo(new File("H:\\cache\\"+originalFilename));
    }

    if(photos.length > 0){
        for (MultipartFile photo : photos) {
            if(!photo.isEmpty()){
                String originalFilename = photo.getOriginalFilename();
                photo.transferTo(new File("H:\\cache\\"+originalFilename));
            }
        }
    }


    return "main";
}
```

#### 3	相关配置

### 6	异常处理

#### 1	默认处理

#### 2	定制错误处理逻辑

- error/404.html   error/5xx.html；有精确的错误状态码页面就匹配精确，没有就找 4xx.html；如果都没有就触发白页

### 7	Web原生组件注入

### 8	嵌入式Servlet容器

### 9	定制化原理

## 数据访问篇

## 单元测试篇

## 指标监控篇等等。。。。。

