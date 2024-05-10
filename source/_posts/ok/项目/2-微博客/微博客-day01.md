## 1 项目概述

### 1.1 项目简介

微博客是一个轻量级的博客项目，项目采用前后端分离开发模式，主要练习前端开发和后端的CURD操作

![image-20230628203120590](img\image-20230628203120590.png)



### 1.2 功能描述

前台：

* 分类列表
* 文章列表

后台：

* 登录和注册

* 文章管理（列表、添加、修改、删除）
* 分类管理（列表、添加、修改、删除）

* 用户中心（查看和修改）



### 1.3 技术描述

* 项目采用前后端分离开发模块，包含前端和后端技术
* 后端技术：SpringBoot、MyBatis、MySQL、Maven、Swagger

* 前端技术：Vue3、ES6、Element Plus、npm



## 2 前后端分离开发

### 2.1 什么是前后端分离开发

前后端分离开发，就是在项目开发过程中，对于前端的代码专门由前端的开发人员开发，后端代码由后端人员负责，这样可以做到分工明确、各司其职，进而提高开发效率，前后端代码并行开发，加快项目的开发进度。目前前后端分离被各大公司使用，成为项目开发的主流开发方式。前后端分离开发后，工程结构也会发生变化，即前后端代码不会混在同一个maven工程中，而是分为前端工程和后端工程。

在美国等互联网环境比较发达的国家项目开发的分工协作更为明确，整个项目开发分为前端、中间层和后端三个开发阶段，这三个阶段分别由三个或者更多的人来协同完成。国内的大部分互联网公司只有前端工程师和后端工程师，中间层的工作有的由前端来完成，有的由后端来完成。



### 2.2 开发流程介绍

![image-20230311092005782](img\前后端分离开发.png)

前后端分离开发过程中，前端人员和后端人员要进行配合来共同完成一个任务。这个时候需要使用到接口。
接口（API接口）：是一个http的请求地址，主要是定义：请求路径、请求方式、请求参数、响应数据等内容。
（1）后端编写和维护接口文档，在 API 变化时更新接口文档
（2）后端根据接口文档进行接口开发
（3）前端根据接口文档进行开发
（4）开发完成后联调和提交测试

后端开发人员为前端提供接口的同时，还需同时提供接口的说明文档。但我们的代码总是会根据实际情况来实时更新，这个时候有可能会忘记更新接口的说明文档，造成一些不必要的问题。我们可以通过一些工具生成接口文档，做到文档实时更新。**Swagger** 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务的接口文档。



## 3 搭建前端环境

### 3.1 准备工作

#### （1）安装Nodejs

#### （2）安装VS Code

### 3.2 初始化VUE项目

####  （1）使用Vite开启Vue3项目

Vite是一个web开发构建工具。

在命令行窗口里，执行npm create vite@latest，就可以初始化一个VUE项目。

在Project name这一行，输入项目名字，例如：learn-vue3。

在Select a framework这一行，选择Vue。

在Select a variant这一行，选择JavaScript。

![image-20230628161326058](img\image-20230628161326058.png)

#### （2）查看项目目录结构

![image-20230628161558697](img\image-20230628161558697.png)

#### （3）安装依赖

在项目目录下，执行npm install 命令，来进行依赖的安装。依赖安装完成后，会在目录下生成一个node_modules目录以及package-lock.json文件。
![image-20230628161737582](img\image-20230628161737582.png)

#### （4）启动项目

执行 npm run dev 命令来启动项目

![image-20230628161840624](img\image-20230628161840624.png)

**查看页面效果：**

![image-20230628162007292](img\image-20230628162007292.png)

### 3.3 安装相关插件和依赖

#### （1）修改package.json文件

```json
{
  "name": "blog-vue-project",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "@wangeditor/editor-for-vue": "^5.1.12",
    "axios": "^0.27.2",
    "element-plus": "^2.3.7",
    "js-cookie": "^3.0.5",
    "pinia": "^2.0.16",
    "sass": "^1.53.0",
    "vue": "^3.2.37",
    "vue-router": "^4.1.2"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^3.0.0",
    "naive-ui": "^2.31.0",
    "unplugin-auto-import": "^0.16.4",
    "unplugin-vue-components": "^0.25.1",
    "vfonts": "^0.0.3",
    "vite": "^3.0.0"
  }
}
```

#### （2）执行npm install下载安装

![image-20230628163337976](img\image-20230628163337976.png)

#### （3）修改vite.config.js文件

```js
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
 
// https://vitejs.dev/config/
export default defineConfig({
  base: "./",
  plugins: [
    vue(),
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

#### （4）修改src下main.js

```js
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import naive from 'naive-ui'
import { createDiscreteApi } from 'naive-ui'
import { router } from './common/router'
import { createPinia } from "pinia";
import axios from 'axios'

import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

import './assets/style.css'

// 服务端地址
axios.defaults.baseURL = "http://localhost:8080"
// 独立API
const { message, notification, dialog } = createDiscreteApi(["message", "dialog", "notification"])


const app = createApp(App)

// 全局提供属性
app.provide("axios", axios)
app.provide("message", message)
app.provide("notification", notification)
app.provide("dialog", dialog)
app.provide("server_url", axios.defaults.baseURL )

app.use(naive)
app.use(createPinia());
app.use(router);
app.use(ElementPlus);

app.mount('#app')
```

### 3.4 创建路由

#### （1）创建router.js文件

![image-20230628164057152](img\image-20230628164057152.png)

```js
import { createRouter, createWebHashHistory } from "vue-router";

let routes = [
    //{ path: "/test", component: () => import("../views/Test.vue") },
    { path: "/", component: () => import("../views/HomePage.vue") },
    { path: "/detail", component: () => import("../views/Detail.vue") },
    { path: "/login", component: () => import("../views/Login.vue") },
    //{ path: "/register", component: () => import("../views/Register.vue") },
    {
        path: "/dashboard", component: () => import("../views/dashboard/Dashboard.vue"), children: [
            { path: "/dashboard/category", component: () => import("../views/dashboard/Category.vue") },
            { path: "/dashboard/article", component: () => import("../views/dashboard/Article.vue") },
            { path: "/dashboard/user", component: () => import("../views/dashboard/User.vue") },
        ]
    },
]

const router = createRouter({
    history: createWebHashHistory(),
    routes,
});

export { router, routes };
```



### 3.5 封装axios

#### （1）创建axios.js

在src下创建文件夹utils，在utils下创建axios.js

![image-20230628164257882](img\image-20230628164257882.png)

```js
import axios from "axios";
// 使用element-plus ElMessage做消息提醒
import { ElMessage } from 'element-plus'

import cookie from 'js-cookie'

//1. 创建新的axios实例，
const service = axios.create({
  // 公共接口 开发环境还是线上环境也可以用api
  baseURL:"http://localhost:8001",
  // 超时时间 单位是ms，这里设置了5s的超时时间
  timeout: 50000,
});
// 2.请求拦截器
service.interceptors.request.use(
  (config) => {
    //发请求前做的一些处理，数据转化，配置请求头，设置token,设置loading等，根据需求去添加
    //  config.data = qs.stringify(config.data); //数据转化,也可以使用qs转换
    //  config.headers = {
    //    'Content-Type':'application/x-www-form-urlencoded' //配置请求头
    //  }
    //注意使用token的时候需要引入cookie方法或者用本地localStorage等方法，推荐js-cookie
    //  const token = getCookie('名称');//这里取token之前，你肯定需要先拿到token,存一下
    //  if(token){
    //     config.params = {'token':token} //如果要求携带在参数中
    //     config.headers.token= token; //如果要求携带在请求头中
    //   }
    // config.method === "post"
    //   ? (config.data = qs.stringify({ ...config.data }))
    //   : (config.params = { ...config.params });
    return config;
  },
  (error) => {
    Promise.reject(error);
  }
);

// 3.响应拦截器
service.interceptors.response.use(
  (response) => {
    if (response.data.code !== 200) {
      ElMessage.error(response.data.message);
      return Promise.resolve(error.response);
    } else {
        return response.data
    }
  },
  (error) => {
    /***** 接收到异常响应的处理开始 *****/
    ElMessage.error("连接服务器失败");
    //ElMessage.error(ElMessage.error)
    /***** 处理结束 *****/
    //如果不需要错误处理，以上的处理过程都可省略
    return Promise.resolve(error.response);
  }
);
//4.导出
export default service;
```

#### （2）创建定义接口信息js文件

在src下创建文件夹api，在api下创建js文件定义接口信息

![image-20230628164518828](img\image-20230628164518828.png)

```js
import request from '../utils/axios.js'

// 编写示例
export const test = (data) => {
    return request({
        url: `/user/test`,
        method: 'post',
        data: data
    })
}
```



### 3.6 整合富文本编辑器

在components文件夹下创建RichTextEditor.vue文件

![image-20230628165031145](img\image-20230628165031145.png)

```vue
<!-- 富文本组件 -->
<template>
    <div>
        <Toolbar :editor="editorRef" :defaultConfig="toolbarConfig" :mode="mode"
            style="border-bottom: 1px solid #ccc" />
        <Editor :defaultConfig="editorConfig" :mode="mode" v-model="valueHtml" style="height: 400px; overflow-y: hidden"
            @onCreated="handleCreated" @onChange="handleChange" />
    </div>
</template>

<script setup>
import '@wangeditor/editor/dist/css/style.css';
import { watch,ref, reactive, inject, onMounted, onBeforeUnmount, shallowRef } from 'vue'
import { Editor, Toolbar } from '@wangeditor/editor-for-vue';

const server_url = inject("server_url")
// 编辑器实例，必须用 shallowRef，重要！
const editorRef = shallowRef();
const toolbarConfig = { excludeKeys:["uploadVideo"] };
const editorConfig = { placeholder: '请输入内容...' };
editorConfig.MENU_CONF = {}
editorConfig.MENU_CONF['uploadImage'] = {
    base64LimitSize: 10 * 1024, // 10kb
    server: server_url+'/upload/rich_editor_upload',
}
editorConfig.MENU_CONF['insertImage'] ={
    parseImageSrc:(src) =>{
        if(src.indexOf("http") !==0){
            return `${server_url}${src}`
        }
        return src
    }
}

const mode = ref("default")
const valueHtml = ref("")

const props = defineProps({
    modelValue: {
        type: String,
        default: ""
    }
})

const emit = defineEmits(["update:model-value"])
let initFinished = false

//监听值的变化，重新赋值
watch(props, (newValue, oldValue) => {
    valueHtml.value = newValue.modelValue
})

onMounted(() => {
    setTimeout(() => {
        valueHtml.value = props.modelValue;
        initFinished = true;
    }, 1000);
});

// 组件销毁时，也及时销毁编辑器，重要！
onBeforeUnmount(() => {
    const editor = editorRef.value;
    if (editor == null) return;
    editor.destroy();
});

// 编辑器回调函数
const handleCreated = (editor) => {
    console.log('created', editor);
    editorRef.value = editor; // 记录 editor 实例，重要！
};
const handleChange = (editor) => {
    if (initFinished) {
        emit("update:model-value", valueHtml.value)
    }
};

</script>

<style lang="scss" scoped>
</style>
```



### 3.7 修改文件

#### （1）修改app.vue

```vue
<template>
  <router-view></router-view>
</template>

<script setup>
</script>

<style scoped>
</style>
```

#### （2）修改style.css

```css
body {
  margin: 0;
  padding: 0;
}
```



### 3.8 创建路由对应的vue文件

#### 3.8.1 复制需要的图片和样式

复制到assets目录下

![image-20230628170622597](img\image-20230628170622597.png)

#### 3.8.2 创建vue页面

**在src下创建views文件夹，在views文件夹下创建对应的vue文件**

![image-20230628165528195](img\image-20230628165528195.png)

#### （1）HomePage.vue

**是项目前台的首页面**

**对应路由：**{ path: "/", component: () => import("../views/HomePage.vue") }, 

```vue
<template>
    <div>
      <header id="header-menu" class="sticky top-0 z-10 flex h-14 bg-white py-3 menu-sticky" x-data="{ open : false }">
        <div class="container mx-auto flex h-full justify-between">
          <div class="flex h-full items-center gap-6">
            <div class="mr-2 h-full">
              <a href="http://127.0.0.1:5173/" class="inline-flex h-full items-center">
                <img src="../assets/atguigu.jpg" alt="Logo" class="h-full w-auto">
              </a>
            </div>
          
            <ul class="hidden items-center gap-8 sm:flex">
              <li class="relative text-sm" x-data="dropdown">
                <a class="text-gray-600" href="http://127.0.0.1:5173/">首页</a>  
              </li>
            </ul>
          </div>
        
          <div class="flex items-center">
              <a href="http://127.0.0.1:5173/#/login" title="发布博文" class="inline-flex h-full items-center">
                <img src="../assets/publish.jpg" alt="Logo" class="h-full w-auto">
                发布文章
              </a>
          </div>
        </div>
      </header>
  
      <section>
        <div class="bg-cover bg-center bg-no-repeat h-96" style="background-image: url(https://www.gulixueyuan.com/files/system/2023/04-04/150831f74f62589616.jpg);height:300px;">
        </div>
      </section>
      
      <section class="container mx-auto mt-6 grid grid-cols-4 gap-6">
        <div class="col-span-4 sm:col-span-3">
        <ul id="filters" class="flex flex-wrap gap-2">
          <li>
            <a href="http://127.0.0.1:5173/">
              <span class="truncate text-base"> 全部 </span>
            </a>
          </li>
        
          <li x-data="dropdown" class="relative cursor-pointer transition-all">
            <a href="http://127.0.0.1:5173/">
            <span class="truncate text-base">默认分类</span>
            </a>
          </li>
          
          <li x-data="dropdown" class="relative cursor-pointer transition-all">
            <a href="http://127.0.0.1:5173/">
            <span class="truncate text-base">MySQL</span>
            </a>
          </li>
  
          <li x-data="dropdown" class="relative cursor-pointer transition-all">
            <a href="http://127.0.0.1:5173/">
            <span class="truncate text-base">vue</span>
            </a>
          </li>
        </ul>
  
        <div id="post-list" class="mt-6 grid grid-cols-1 gap-6  md:grid-cols-2">
          <div class="overflow-hidden rounded-xl bg-white shadow-md  hover:-translate-y-1 hover:ring-2 ">
            <div class="aspect-w-16 aspect-h-9">
            <a href="http://127.0.0.1:5173/#/detail" title="【spring MVC】">
              <img src="https://www.gulixueyuan.com/files/course/2021/08-02/151735f51c11714475.png" class="h-full w-full object-cover transition-all duration-500 group-hover:scale-105">
            </a>
            </div>
            <div class="relative flex flex-col gap-2 p-4">
            <h1 class="text-2xl font-medium">
              <a href="http://127.0.0.1:5173/#/detail" title="【spring MVC】">【spring MVC】</a>
            </h1>
            <p class="font-sm font-light line-clamp-6">Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用Spring进行WEB开发时，可以选择使用Spring的Spring MVC框架或集成其他MVC开发框架</p>
            <div class="mt-4 flex flex-1 items-center">
              <a href="http://127.0.0.1:5173/#/detail">
              <img src="../assets/atguigu.jpg" class="h-8 w-8 dark:border-slate-700">
              </a>
              <span class="text-sm text-gray-600">发布于 2023-06-26</span>
            </div>
            </div>
          </div>
            
          <div class="overflow-hidden rounded-xl bg-white shadow-md  hover:-translate-y-1 hover:ring-2 ">
            <div class="aspect-w-16 aspect-h-9">
            <a href="http://127.0.0.1:5173/#/detail" title="【spring MVC】">
              <img src="https://www.gulixueyuan.com/files/course/2021/08-02/151735f51c11714475.png" class="h-full w-full object-cover transition-all duration-500 group-hover:scale-105">
            </a>
            </div>
            <div class="relative flex flex-col gap-2 p-4">
            <h1 class="text-2xl font-medium">
              <a href="http://127.0.0.1:5173/#/detail" title="【spring MVC】">【spring MVC】</a>
            </h1>
            <p class="font-sm font-light line-clamp-6">Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用Spring进行WEB开发时，可以选择使用Spring的Spring MVC框架或集成其他MVC开发框架</p>
            <div class="mt-4 flex flex-1 items-center">
              <a href="http://127.0.0.1:5173/#/detail">
              <img src="../assets/atguigu.jpg" class="h-8 w-8 dark:border-slate-700">
              </a>
              <span class="text-sm text-gray-600">发布于 2023-06-26</span>
            </div>
            </div>
          </div>
        </div>
      </div>
        
      <aside class="col-span-1 flex-col gap-6 sm:flex">
        <div class="bg-white p-3 shadow transition-all">
          <div class="flex flex-col items-center ">
          <div class="relative h-24 w-24">
            <img src="../assets/01.jpg" alt="Java程序员" class="h-full w-full rounded-full">
          </div>
          <div><h1 class="text-2xl">Java程序员</h1></div>
          
          <div class="grid grid-cols-4 gap-5">
            <div class="inline-flex flex-col items-center">
            <span class="text-xl">33</span>
            <span class="text-xs">文章数</span>
            </div>
            <div class="inline-flex flex-col items-center">
            <span class="text-xl">26</span>
            <span class="text-xs">分类数</span>
            </div>
          
            <div class="inline-flex flex-col items-center">
            <span class="text-xl">23403</span>
            <span class="text-xs">访问量</span>
            </div>
          </div>
          </div>
        </div>
  
        <div class="bg-white p-3 shadow">
          <h2>
            <span class="text-lg"></span>
            热门文章
          </h2>
        <div>
          <ul role="list">
            <li class="py-3">
              <div class="flex items-center">
                <div class="flex flex-col gap-1">
                  <h3 >
                    <a href="#" title="【MyBatis】MyBatis 和 Hibernate">【MyBatis】MyBatis 和 Hibernate</a>
                  </h3>
                  <p>2023-06-26 发布</p>
                </div>
              </div>
            </li>
            <li class="py-3">
              <div class="flex items-center">
                <div class="flex flex-col gap-1">
                  <h3>
                    <a href="#" title="【对比】Native ui 和 Element Plus">【【对比】Native ui 和 Element Plus</a>
                  </h3>
                  <p>2023-06-13 发布</p>
                </div>
              </div>
            </li>
            <li class="py-3">
              <div class="flex items-center">
                <div class="flex flex-col gap-1">
                  <h3 >
                    <a href="#" title="【毕业季】毕业设计指南">【毕业季】毕业设计指南</a>
                  </h3>
                  <p>2023-05-23 发布</p>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
      </aside>
      </section>
    </div>
  </template>
  
  <style lang="scss" scoped>
  .login-panel {
      width: 500px;
      margin: 0 auto;
      margin-top: 130px;
  }
  </style>
```

#### （2）Detail.vue

文章详情页面

```vue
<template>
    <div>
      <header id="header-menu" class="sticky top-0 z-10 flex h-14 bg-white py-3 menu-sticky" x-data="{ open : false }">
        <div class="container mx-auto flex h-full justify-between">
          <div class="flex h-full items-center gap-6">
            <div class="mr-2 h-full">
              <a href="http://127.0.0.1:5173/" class="inline-flex h-full items-center">
                <img src="../assets/atguigu.jpg" alt="Logo" class="h-full w-auto">
              </a>
            </div>
          
            <ul class="hidden items-center gap-8 sm:flex">
              <li class="relative text-sm" x-data="dropdown">
                <a class="text-gray-600" href="http://127.0.0.1:5173/">首页</a>  
              </li>
            </ul>
          </div>
        
          <div class="flex items-center">
              <a href="http://127.0.0.1:5173/#/login" title="发布博文" class="inline-flex h-full items-center">
                <img src="../assets/publish.jpg" alt="Logo" class="h-full w-auto">
                发布文章
              </a>
          </div>
        </div>
      </header>
      
      <section class="container mx-auto mt-6 grid grid-cols-4 gap-6">
        <div class="col-span-4 sm:col-span-3">
  
      <div x-data="postUpvote" class="rounded-xl bg-white p-4 dark:bg-slate-800">
        <div class="flex items-center justify-between">
          <div class="inline-flex items-center justify-start gap-2">
            <div class="flex flex-col gap-0.5">
              <a href="https://www.javadog.net/authors/hdxjhkhjc2012" title="Java程序员" class="text-sm font-semibold text-gray-900 hover:text-gray-600 dark:text-slate-100 dark:hover:text-slate-200">Java程序员</a>
              <div class="flex items-center gap-2 text-xs font-light text-gray-600 dark:text-slate-100">
                <span>发布于 2023-05-23</span>
              </div>
            </div>
          </div>
          
        </div>
        <h1 class="my-3 text-2xl font-medium dark:text-slate-50">【毕业季】毕业设计避坑指南</h1>
        
        <article id="content" class="prose prose-base mt-4 !max-w-none prose-pre:p-0 dark:prose-invert break-words"><h2 id="前言">前言</h2>
  
        <h4 id="毕业季，毕业设计分手季">毕业季，毕业设计分手季</h4>
        <p>每年到了初夏的季节，校园美好青春即将落幕。然而挡在毕业前的最大的障碍不是<strong>分手</strong>，而是<strong>毕业设计和毕业答辩</strong>。有些同学可能会对<strong>毕业设计无从下手</strong>，从而<strong>盲目选题</strong>，或者听从网上购买毕设导致被欺骗，最终<strong>影响毕业</strong>。本狗深知其中套路，特来献出<strong>避坑指南</strong>，希望略尽绵薄之力，帮助学子们跨过深坑。</p>
        </article>
      </div>
  
      </div>
        
      <aside class="col-span-1 flex-col gap-6 sm:flex">
        <div class="bg-white p-3 shadow transition-all">
          <div class="flex flex-col items-center ">
          <div class="relative h-24 w-24">
            <img src="../assets/01.jpg" alt="Java程序员" class="h-full w-full rounded-full">
          </div>
          <div><h1 class="text-2xl">Java程序员</h1></div>
          
          <div class="grid grid-cols-4 gap-5">
            <div class="inline-flex flex-col items-center">
            <span class="text-xl">33</span>
            <span class="text-xs">文章数</span>
            </div>
            <div class="inline-flex flex-col items-center">
            <span class="text-xl">26</span>
            <span class="text-xs">分类数</span>
            </div>
          
            <div class="inline-flex flex-col items-center">
            <span class="text-xl">23403</span>
            <span class="text-xs">访问量</span>
            </div>
          </div>
          </div>
        </div>
  
        <div class="bg-white p-3 shadow">
          <h2>
            <span class="text-lg"></span>
            热门文章
          </h2>
        <div>
          <ul role="list">
            <li class="py-3">
              <div class="flex items-center">
                <div class="flex flex-col gap-1">
                  <h3 >
                    <a href="#" title="【MyBatis】MyBatis 和 Hibernate">【MyBatis】MyBatis 和 Hibernate</a>
                  </h3>
                  <p>2023-06-26 发布</p>
                </div>
              </div>
            </li>
            <li class="py-3">
              <div class="flex items-center">
                <div class="flex flex-col gap-1">
                  <h3>
                    <a href="#" title="【对比】Native ui 和 Element Plus">【【对比】Native ui 和 Element Plus</a>
                  </h3>
                  <p>2023-06-13 发布</p>
                </div>
              </div>
            </li>
            <li class="py-3">
              <div class="flex items-center">
                <div class="flex flex-col gap-1">
                  <h3 >
                    <a href="#" title="【毕业季】毕业设计指南">【毕业季】毕业设计指南</a>
                  </h3>
                  <p>2023-05-23 发布</p>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
      </aside>
      </section>
    </div>
  </template>
```

#### （3）Login.vue

发布文章需要登录后台，是登录页面

```vue
<template>
  <div class="login">
    <el-form class="form" :model="user" :rules="formRules" ref="userForm">
      <h1 class="title">微博客后台管理系统</h1>
      <el-form-item prop="username">
        <el-input
          class="text"
          v-model="user.username"
          prefix-icon="User"
          clearable
          placeholder="用户名"
        />
      </el-form-item>
      <el-form-item prop="password">
        <el-input
          class="text"
          v-model="user.password"
          prefix-icon="Lock"
          show-password
          clearable
          placeholder="密码"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          :loading="loading"
          type="primary"
          class="btn"
          size="large"
          @click="loginUser"
        >
          登录
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
  
<script setup>
  import { ref , onMounted } from 'vue';
  import { useRouter, useRoute } from 'vue-router'
  const router = useRouter()
  const route = useRoute()
  
  const defaultForm = {
      username: "",
      password: ""
  }
  
  const user = ref(defaultForm) 
  
  const userForm = ref(null)
  
  //分页数据
  const pageParamsForm = {
      username: [
          { required: true, message: "请输入账号", trigger: "blur" },
          { min: 3, max: 12, message: "账号长度在 3 到 12 个字符", trigger: "blur" },
      ],
      password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 18, message: "密码长度在 6 到 18 个字符", trigger: "blur" },
      ],
  }
  const formRules = ref(pageParamsForm) 

  const loginUser = () => {
    router.push("/dashboard/article")
  }
</script>
  
<style lang="scss" scoped>
  .login {
    transition: transform 1s;
    transform: scale(1);
    width: 100%;
    height:600px;
      margin: 0 auto;
    overflow: hidden;
    background: #2d3a4b;
  .form {
    width: 520px;
    max-width: 100%;
    padding: 0 24px;
    box-sizing: border-box;
    margin: 160px auto 0;
    :deep {
      .el-input__wrapper {
        box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1) inset;
        background: rgba(0, 0, 0, 0.1);
      }
      .el-input-group--append > .el-input__wrapper {
        border-top-right-radius: 0;
        border-bottom-right-radius: 0;
      }
      .el-input-group--prepend > .el-input__wrapper {
        border-top-left-radius: 0;
        border-bottom-left-radius: 0;
      }
    }
    .title {
      color: #fff;
      text-align: center;
      font-size: 24px;
      margin: 0 0 24px;
    }
    .text {
      font-size: 16px;
      :deep(.el-input__inner) {
        color: #fff;
        height: 48px;
        line-height: 48px;
        &::placeholder {
          color: rgba(255, 255, 255, 0.2);
        }
      }
    }
    .btn {
      width: 100%;
    }
    }
  }
</style>
```

#### （4）Dashboard.vue

后台系统的主界面

```vue
<template>
    <div class="main-panel">
        <div class="menus">
            当前用户：
            <el-button type="primary">
                {{name}}
            </el-button>
            
            <div v-for="(menu, index) in menus" @click="toPage(menu)">
                {{ menu.name }}
            </div>
            
        </div>
        <div style="padding:20px;width:80%">
            <router-view></router-view>
        </div>
    </div>
    <div class="title">后台管理系统</div>
</template>

<script setup>
import { ref, reactive, inject, onMounted, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import cookie from 'js-cookie'

const router = useRouter()
const route = useRoute()

// 分类列表
const props = [
    { name: "文章管理", href: "/dashboard/article" },
    { name: "分类管理", href: "/dashboard/category" },
    { name: "用户中心", href: "/dashboard/user" },
    { name: "退出", href: "logout" },
]
const menus = ref(props)

const name = ref(null)

onMounted(() => {
    getName()
})

const getName = ()=>{
    name.value = cookie.get('name')
    // if(!name.value) {
    //     router.push("/login")
    // }
}

const toPage = (menu) => {
    if (menu.href == 'logout') {
        router.push("/login")
    } else {
        let paths = menu.href;
        router.push(paths)
    }
}
</script>

<style lang="scss" scoped>
.main-panel {
    display: flex;
    color: #64676a;
    max-width: 100%;
}

.menus {
    line-height: 55px;
    text-align: center;
    width: 20%;
    height: 100%vh;
    border-right: 2px solid #dadada;
    cursor: pointer;
}

.title {
    font-size: 65px;
    font-weight: bold;
    text-align: right;
    position: fixed;
    color: rgba(0, 0, 0, 20%);
    right: calc((100vw - 1500px) / 2);
    bottom: 20px;
}
</style>
```

#### （5）Article.vue

文章管理页面，包含文章列表、添加、查看和删除

```vue
<template>
    <div class="search-div">

        <!-- 添加按钮 -->
        <div class="tools-div">
            <el-button type="success" size="small" @click="addShow">添 加</el-button>
        </div>
        
        <!--- 角色表格数据 -->
        <el-table :data="list" style="width: 100%">
            <el-table-column prop="id" label="文章ID" width="180" />
            <el-table-column prop="title" label="文章标题" width="180" />
            <el-table-column prop="createTime" label="发布时间" />
            <el-table-column label="操作" align="center" width="280" #default="scope">
                <el-button type="primary" size="small" @click="show(scope.row)">
                    查看
                </el-button>
                <el-button type="danger" size="small" @click="deleteById(scope.row)">
                    删除
                </el-button>
            </el-table-column>
        </el-table>

        <el-dialog v-model="dialogVisible" title="添加或修改文章" width="60%">
            <el-form label-width="100px">
            <el-form-item label="文章标题">
                <el-input v-model="article.title"/>
            </el-form-item>

            <el-form-item label="所属分类">
                <el-select
                class="m-2"
                v-model="article.cid"
                placeholder="选择分类"
                size="small"
                style="width: 100%"
                >
                <el-option
                    v-for="item in categoryList"
                    :key="item.id"
                    :label="item.cname"
                    :value="item.cid"
                />
                </el-select>
          </el-form-item>

            <el-form-item label="文章内容">
                <rich-text-editor v-model="article.content"></rich-text-editor>
            </el-form-item>

            <el-form-item>
                <el-button type="primary" @click="submit">提交</el-button>
                <el-button @click="dialogVisible = false">取消</el-button>
            </el-form-item>
            </el-form>
        </el-dialog>

        <el-dialog v-model="dialogShowVisible" title="文章详情" width="60%">
            <el-tag>{{article.title}}</el-tag>
            <div v-html="article.content"></div>
        </el-dialog>

  </div>

</template>

<script setup>
import { ref , onMounted } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus'
import RichTextEditor from '../../components/RichTextEditor.vue'

const categoryList = ref([])
// 定义表格数据模型
let list = ref([])

// 控制对话是否展示的变量
const dialogVisible = ref(false)
const dialogShowVisible = ref(false)
const defaultForm = {
    id: "",
    cid:"",
    title: "",
    content:""
}
const article = ref(defaultForm)   // 使用ref包裹该对象，使用reactive不方便进行重置

</script>

<style scoped>

.search-div {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ebeef5;
  border-radius: 3px;
  background-color: #fff;
}

.tools-div {
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ebeef5;
  border-radius: 3px;
  background-color: #fff;
}

</style>
```

#### （6）Category.vue

分类管理页面，包含列表、添加、修改、删除

```vue
<template>
    <div class="search-div">
        <!-- 搜索表单 -->
        <el-form label-width="70px" size="small">
            <el-form-item label="分类名称">
                <el-input
                v-model="queryDto.cname"
                style="width: 100%"
                placeholder="角色名称"
                ></el-input>
            </el-form-item>
            <el-row style="display:flex">
                <el-button type="primary" size="small" @click="searchCategory">
                搜索
                </el-button>
                <el-button size="small" @click="resetData">重置</el-button>
            </el-row>
        </el-form>

        <!-- 添加按钮 -->
        <div class="tools-div">
            <el-button type="success" size="small" @click="addShow">添 加</el-button>
        </div>
        
        <!--- 角色表格数据 -->
        <el-table :data="list" style="width: 100%">
            <el-table-column prop="cid" label="分类id" width="180" />
            <el-table-column prop="cname" label="分类名称" width="180" />
            <el-table-column prop="createTime" label="创建时间" />
            <el-table-column label="操作" align="center" width="280" #default="scope">
                <el-button type="primary" size="small" @click="editShow(scope.row)">
                    修改
                </el-button>
                <el-button type="danger" size="small" @click="deleteById(scope.row)">
                    删除
                </el-button>
            </el-table-column>
        </el-table>

        <!--分页条-->
        <el-pagination
            v-model:current-page="pageParams.page"
            v-model:page-size="pageParams.limit"
            @size-change="fetchData"
            @current-change="fetchData"
            layout="total, prev, pager, next"
            :total="total"
        />

        <el-dialog v-model="dialogVisible" title="添加或修改" width="30%">
            <el-form label-width="120px">
            <el-form-item label="分类名称">
                <el-input v-model="category.cname"/>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submit">提交</el-button>
                <el-button @click="dialogVisible = false">取消</el-button>
            </el-form-item>
            </el-form>
        </el-dialog>
  </div>

</template>

<script setup>
import { ref , onMounted } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus'

// 分页条总记录数
let total = ref(0)

//分页数据
const pageParamsForm = {
  page: 1, // 页码
  limit: 2, // 每页记录数
}
const pageParams = ref(pageParamsForm)     // 将pageParamsForm包装成支持响应式的对象

// 定义表格数据模型
let list = ref([])

// 搜索表单数据
const queryDto = ref({"cname": ""})

// 控制对话是否展示的变量
const dialogVisible = ref(false)
const defaultForm = {
    cid: "",
    cname: ""
}
const category = ref(defaultForm)   // 使用ref包裹该对象，使用reactive不方便进行重置

</script>

<style scoped>

.search-div {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ebeef5;
  border-radius: 3px;
  background-color: #fff;
}

.tools-div {
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ebeef5;
  border-radius: 3px;
  background-color: #fff;
}

</style>
```

#### （7）User.vue

```vue
<template>
    <div class="login-panel">
        <el-form :rules="formRules" :model="userInfo" ref="userForm">
            <el-form-item path="username" label="账号">
                <el-input :disabled="true" v-model="userInfo.username" placeholder="请输入账号" />
            </el-form-item>
            <el-form-item path="password" label="密码">
                <el-input :disabled="true" v-model="userInfo.password" type="password" placeholder="请输入密码" />
            </el-form-item>
            <el-form-item path="phone" label="手机号">
                <el-input v-model="userInfo.phone" placeholder="请输入手机号" />
            </el-form-item>

            <el-form-item path="introduction" label="简介">
                <el-input
                v-model="userInfo.introduction"
                placeholder="个人简介"
                type="textarea"
                :autosize="{
                    minRows: 3,
                    maxRows: 5
                }"
                />
            </el-form-item> 

            <el-form-item>
                <el-button type="error" @click="modifyUser">
                修改
                </el-button>
            </el-form-item> 
        </el-form>
    </div>
</template>

<script setup>
import { ref, reactive, inject, onMounted, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { ElMessage, ElMessageBox } from 'element-plus'

const router = useRouter()
const route = useRoute()

// 分类列表
const props = {
  uid:null,
  username:'',
  password:'',
  phone:'',
  introduction:''
};
const userInfo = ref(props)

</script>

<style lang="scss" scoped>
.login-panel {
    width: 400px;
    margin: 0;
}
</style>
```



## 4 搭建后端环境

### 4.1 创建工程

![image-20230627215944838](img\image-20230627215944838.png)



### 4.2 引入依赖

在blog_project引入依赖

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

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.30</version>
    </dependency>
</dependencies>
```



### 4.3 创建数据库和表

![image-20230625110247990](img\image-20230625110247990.png)

```sql
/*
SQLyog Ultimate - MySQL GUI v8.2 
MySQL - 8.0.30 : Database - blog
*********************************************************************
*/


/*!40101 SET NAMES utf8 */;

/*!40101 SET SQL_MODE=''*/;

/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
CREATE DATABASE /*!32312 IF NOT EXISTS*/`blog` /*!40100 DEFAULT CHARACTER SET utf8mb3 */ /*!80016 DEFAULT ENCRYPTION='N' */;

USE `blog`;

/*Table structure for table `article` */

DROP TABLE IF EXISTS `article`;

CREATE TABLE `article` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `title` varchar(100) DEFAULT NULL,
  `content` text,
  `cid` bigint DEFAULT NULL,
  `uid` bigint DEFAULT NULL,
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=28 DEFAULT CHARSET=utf8mb3;

/*Data for the table `article` */

insert  into `article`(`id`,`title`,`content`,`cid`,`uid`,`create_time`,`update_time`) values (9,'Spring轻量级框架','spring',2,1,'2023-06-28 12:27:41','2023-06-28 12:27:41'),(11,'Vue前端框架','vue',2,1,'2023-06-28 12:30:17','2023-06-28 12:30:17'),(19,'Java','java',3,1,'2023-07-11 20:20:45','2023-07-11 20:20:45'),(27,'9999',NULL,NULL,NULL,'2023-07-12 14:12:11','2023-07-12 14:12:11');

/*Table structure for table `category` */

DROP TABLE IF EXISTS `category`;

CREATE TABLE `category` (
  `cid` bigint NOT NULL AUTO_INCREMENT,
  `cname` varchar(100) DEFAULT NULL,
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`cid`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8mb3;

/*Data for the table `category` */

insert  into `category`(`cid`,`cname`,`create_time`,`update_time`) values (2,'java','2023-06-28 09:59:54','2023-06-28 09:49:54'),(3,'mysql','2023-06-28 09:50:03','2023-06-28 09:50:03'),(9,'vue','2023-07-11 20:28:31','2023-07-11 20:28:31');

/*Table structure for table `user` */

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `uid` bigint NOT NULL AUTO_INCREMENT,
  `username` varchar(20) DEFAULT NULL,
  `password` varchar(20) DEFAULT NULL,
  `phone` varchar(20) DEFAULT NULL,
  `introduction` varchar(255) DEFAULT NULL,
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`uid`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb3;

/*Data for the table `user` */

insert  into `user`(`uid`,`username`,`password`,`phone`,`introduction`,`create_time`,`update_time`) values (1,'lucy','123456','13567890987','334666777','2023-06-28 10:31:48','2023-06-28 10:31:48'),(2,'1','1','1','1','2023-07-11 20:37:19','2023-07-11 20:37:19');

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
```



### 4.4 创配置文件

application.properties

```properties
server.port=8001

spring.datasource.url=jdbc:mysql://localhost:3306/blog
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.type=com.zaxxer.hikari.HikariDataSource

mybatis.mapper-locations=classpath:/mapper/*.xml
mybatis.configuration.map-underscore-to-camel-case=true

spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=GMT+8
```



### 4.5 创建启动类

```java
package com.atguigu.blog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BlogApplication {

    public static void main(String[] args) {
        SpringApplication.run(BlogApplication.class,args);
    }
}
```



### 4.6 创建实体类

![image-20230627220850377](img\image-20230627220850377.png)

#### （1）文章实体类：Article

```java
package com.atguigu.blog.entity;

import java.util.Date;

public class Article {
    
    private Long id;
    private String title; //文章标题
    private String content; //文章内容
    private Long cid; //分类id
    private Long uid; //用户id
    private Date create_time;
    private Date update_time;

    public Long getId() {
        return id;
    }
    public void setId(Long id) {
        this.id = id;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
    public Long getCid() {
        return cid;
    }
    public void setCid(Long cid) {
        this.cid = cid;
    }
    public Long getUid() {
        return uid;
    }
    public void setUid(Long uid) {
        this.uid = uid;
    }
    public Date getCreate_time() {
        return create_time;
    }
    public void setCreate_time(Date create_time) {
        this.create_time = create_time;
    }
    public Date getUpdate_time() {
        return update_time;
    }
    public void setUpdate_time(Date update_time) {
        this.update_time = update_time;
    }
}
```

#### （2）分类实体类：Category

```java
package com.atguigu.blog.entity;

import java.util.Date;

public class Category {
    
    private Long cid;
    private String cname; //分类名称
    private Date create_time;
    private Date update_time;

    public Long getCid() {
        return cid;
    }
    public void setCid(Long cid) {
        this.cid = cid;
    }
    public String getCname() {
        return cname;
    }
    public void setCname(String cname) {
        this.cname = cname;
    }
    public Date getCreate_time() {
        return create_time;
    }
    public void setCreate_time(Date create_time) {
        this.create_time = create_time;
    }
    public Date getUpdate_time() {
        return update_time;
    }
    public void setUpdate_time(Date update_time) {
        this.update_time = update_time;
    }
}
```

#### （3）用户实体类：User

```java
package com.atguigu.blog.entity;

import java.util.Date;

public class User {
    
    private Long uid; //用户id
    private String username;//用户名称
    private String password;//密码
    private String phone;//手机号
    private String introduction;//个人介绍
    private Date create_time;
    private Date update_time;

    public Long getUid() {
        return uid;
    }
    public void setUid(Long uid) {
        this.uid = uid;
    }
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
    public String getPhone() {
        return phone;
    }
    public void setPhone(String phone) {
        this.phone = phone;
    }
    public String getIntroduction() {
        return introduction;
    }
    public void setIntroduction(String introduction) {
        this.introduction = introduction;
    }
    public Date getCreate_time() {
        return create_time;
    }
    public void setCreate_time(Date create_time) {
        this.create_time = create_time;
    }
    public Date getUpdate_time() {
        return update_time;
    }
    public void setUpdate_time(Date update_time) {
        this.update_time = update_time;
    }
}
```



### 4.7 创建controller

![image-20230627222230344](img\image-20230627222230344.png)



### 4.8 创建service

![image-20230627222549996](img\image-20230627222549996.png)



### 4.9 创建mapper

#### （1）创建mapper接口

![image-20230627222756719](img\image-20230627222756719.png)

#### （2）创建mapper映射文件

![image-20230627222922940](img\image-20230627222922940.png)

### 4.10 统一结果返回类

项目中我们会将响应封装成json返回，一般我们会将所有接口的数据格式统一， 使前端(iOS Android, Web)对数据的操作更一致、轻松。

一般情况下，统一返回数据格式没有固定的格式，只要能描述清楚返回的数据状态以及要返回的具体数据就可以。但是一般会包含状态码、返回消息、数据这几部分内容

例如，我们的系统要求返回的基本数据格式如下：

**列表：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "id": 1,
    "title": "java开发",
    "content": "java是面向对象语言",
    "cid": 3,
    "uid": 1
  },
  "ok": true
}
```

**分页：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "total": 3,
    "list": [
      {
        "id": 1,
        "title": "java开发",
        "content": "java是面向对象语言",
        "cid": 3,
        "uid": 1
      },
      {
        "id": 2,
        "title": "spring",
        "content": "spring是一款轻量级框架",
        "cid": 1,
        "uid": 10
      }
    ]
  },
  "ok": true
}
```

**没有返回数据：**

```json
{
  "code": 200,
  "message": "成功",
  "data": null,
  "ok": true
}
```

**失败：**

```json
{
  "code": 201,
  "message": "失败",
  "data": null,
  "ok": false
}
```



#### （1）统一返回结果状态类 

```java
package com.atguigu.blog.utils;

import lombok.Getter;

/**
 * 统一返回结果状态信息类
 *
 */
@Getter
public enum ResultCodeEnum {

    SUCCESS(200,"成功"),
    FAIL(201, "失败"),
    ;

    private Integer code;
    private String message;
    private ResultCodeEnum(Integer code, String message) {
        this.code = code;
        this.message = message;
    }
}
```



#### （2）创建统一返回结果类

```java
package com.atguigu.blog.utils;

import lombok.Data;

/**
 * 全局统一返回结果类
 *
 */
@Data
public class Result<T> {

    private Integer code;

    private String message;

    private T data;

    public Result(){}

    // 返回数据
    protected static <T> Result<T> build(T data) {
        Result<T> result = new Result<T>();
        if (data != null)
            result.setData(data);
        return result;
    }

    public static <T> Result<T> build(T body, Integer code, String message) {
        Result<T> result = build(body);
        result.setCode(code);
        result.setMessage(message);
        return result;
    }

    public static <T> Result<T> build(T body, ResultCodeEnum resultCodeEnum) {
        Result<T> result = build(body);
        result.setCode(resultCodeEnum.getCode());
        result.setMessage(resultCodeEnum.getMessage());
        return result;
    }

    public static<T> Result<T> ok(){
        return Result.ok(null);
    }

    /**
     * 操作成功
     * @param data  baseCategory1List
     * @param <T>
     * @return
     */
    public static<T> Result<T> ok(T data){
        Result<T> result = build(data);
        return build(data, ResultCodeEnum.SUCCESS);
    }

    public static<T> Result<T> fail(){
        return Result.fail(null);
    }

    /**
     * 操作失败
     * @param data
     * @param <T>
     * @return
     */
    public static<T> Result<T> fail(T data){
        Result<T> result = build(data);
        return build(data, ResultCodeEnum.FAIL);
    }

    public Result<T> message(String msg){
        this.setMessage(msg);
        return this;
    }

    public Result<T> code(Integer code){
        this.setCode(code);
        return this;
    }

    public boolean isOk() {
        if(this.getCode().intValue() == ResultCodeEnum.SUCCESS.getCode().intValue()) {
            return true;
        }
        return false;
    }
}
```



