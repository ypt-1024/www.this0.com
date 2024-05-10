## 9 统一异常处理

### 9.1 制造异常

除以0

```java
int a = 10/0;
```

 出现500错误提示

```json
{
  "timestamp": "2023-07-10 11:32:33",
  "status": 500,
  "error": "Internal Server Error",
  "path": "/article/getAllData"
}
```

我们想让异常结果也显示为统一的返回结果对象，并且统一处理系统的异常信息，那么需要统一异常处理



### 9.2 创建统一异常处理器

创建统一异常处理类ExceptionOperation.java：

```java
import com.atguigu.blog.utils.Result;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

@ControllerAdvice
public class ExceptionOperation {

    @ExceptionHandler(Exception.class)
    @ResponseBody
    public Result error(Exception e) {
        e.printStackTrace();
        return Result.fail().message("出错了...");
    }
}

```

### 9.3 测试 

返回统一错误结果

```json
{
  "code": 201,
  "message": "出错了...",
  "data": null
}
```



## 10 前后端功能整合

### 10.1 定义接口信息

#### （1）在api目录下创建js文件

![image-20230628203700334](img\image-20230628203700334.png)

#### （2）article.js

定义文章相关接口信息，包含接口路径、提交方式、参数信息

```js
import request from '../utils/axios.js'

// 列表
export const listArticle = (article) => {
    return request({
        url: `/article/queryList`,
        method: 'post',
        data: article
    })
}

// 新增
export const addArticle = (article) => {
    return request({
        url: "/article/addArticle",
        method: "post",
        data: article
    })
}

// 根据id查询
export const getArticle = (id) => {
    return request({
        url: `/article/getArticle/${id}`,
        method: "get"
    })
}

// 修改
export const updateArticle = (article) => {
    return request({
        url: "/article/updateArticle",
        method: "put",
        data: article
    })
}

// 删除
export const deleteArticle = (id) => {
    return request({
        url: `/article/deleteArticle/${id}`,
        method: "delete"
    })
}
```

#### （3）category.js

定义分类相关接口信息，包含接口路径、提交方式、参数信息

```javascript
import request from '../utils/axios.js'

// 分类列表
export const listCategory = () => {
    return request({
        url: `/category/queryList`,
        method: 'post'
    })
}

// 条件分页分类列表
export const listCategoryPage = (current,limit,category) => {
    return request({
        url: `/category/queryListPage/${current}/${limit}`,
        method: 'post',
        data: category
    })
}

// 新增
export const addCategory = (category) => {
    return request({
        url: "/category/addCategory",
        method: "post",
        data: category
    })
}

// 根据id查询
export const getCategory = (id) => {
    return request({
        url: `/category/getCategory/${id}`,
        method: "get"
    })
}

// 修改
export const updateCategory = (category) => {
    return request({
        url: "/category/updateCategory",
        method: "put",
        data: category
    })
}

// 删除
export const deleteCategory = (id) => {
    return request({
        url: `/category/deleteCategory/${id}`,
        method: "delete"
    })
}
```

#### （4）user.js

定义用户相关接口信息，包含接口路径、提交方式、参数信息

```js
import request from '../utils/axios.js'

// 登录
export const login = (user) => {
    return request({
        url: `/user/login`,
        method: 'post',
        data: user
    })
}

// 注册
export const register = (user) => {
    return request({
        url: "/user/register",
        method: "post",
        data: user
    })
}

// 根据id查询
export const getUser = (id) => {
    return request({
        url: `/user/getUser/${id}`,
        method: "get"
    })
}

// 修改
export const updateUser = (user) => {
    return request({
        url: "/user/updateUser",
        method: "put",
        data: user
    })
}
```



### 10.2 前台-文章和分类列表

#### （1）在HomePage.vue实现调用

```vue
<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { listArticle , addArticle , getArticle , updateArticle , deleteArticle } from '../api/article';
import { listCategory } from '../api/category';

const router = useRouter()

//定义分类和文章集合
const categoryList = ref([])
const articleList = ref([])

//定义分类id查询条件
const prop = {
    cid: null
}
const categoryData = ref(prop) 

//钩子函数
onMounted(() => {
    getCategortyList(),
    getArticleList()
})

//页面跳转到文章详情页面
const showArticle = (id) => {
    router.push({ path: "/detail",query:{id:id}})
}

//根据分类查询文章
const selectArticle = (cid) => {
    categoryData.value.cid = cid
    getArticleList()
}

//查询所有分类
const getCategortyList = async () => {
    const { data } = await listCategory()
    categoryList.value = data
}

//查询所有文章
const getArticleList = async () => {
    const { data } = await listArticle(categoryData.value)
    articleList.value = data
}
</script>
```

#### （2）在HomePage.vue处理页面

* 显示所有分类

```vue
<li v-for="(category, index)  in categoryList" x-data="dropdown" class="relative cursor-pointer transition-all">
  <a href="#" @click="selectArticle(category.cid)">
  <span class="truncate text-base">{{category.cname}}</span>
  </a>
</li>
```

* 显示所有文章

```vue
<div v-for="article in articleList" class="overflow-hidden rounded-xl bg-white shadow-md  hover:-translate-y-1 hover:ring-2 ">
  <div  @click="showArticle(article.id)" class="aspect-w-16 aspect-h-9">
      <img src="https://www.gulixueyuan.com/files/course/2021/08-02/151735f51c11714475.png" class="h-full w-full object-cover transition-all duration-500 group-hover:scale-105">
  </div>

  <div class="relative flex flex-col gap-2 p-4">
      <h1 @click="showArticle(article.id)">
        {{article.title}}
      </h1>
      <p class="font-sm font-light line-clamp-6 dark:text-slate-200"
         v-html="article.content"></p>
      <div class="mt-4 flex flex-1 items-center justify-start gap-2">
        <a href="#" >
          <img src="../assets/atguigu.jpg"  class="h-8 w-8 dark:border-slate-700">
        </a>
        <span class="text-sm text-gray-600">发布于 {{article.createTime}}</span>
      </div>
  </div>
</div>
```



### 10.3 前台-文章详情

#### （1）添加详情跳转方法

```vue
<h1 @click="showArticle(article.id)">
	{{article.title}}
</h1>
```

#### （2）实现详情跳转方法

```js
//页面跳转到文章详情页面
const showArticle = (id) => {
    router.push({ path: "/detail",query:{id:id}})
}
```

#### （3）Detail.vue编写调用

```vue
<script setup>
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { getArticle } from '../api/article';

const router = useRouter()
const route = useRoute()

// 文章详情
const props = {
  id:'',
  title:'',
  content:''
};
const articleInfo = ref(props)

onMounted(() => {
    loadArticle()
})

/**
 * 读取文章详情
 */
const loadArticle = async () => {
    let id = route.query.id
    const { data } = await getArticle(id)
    articleInfo.value = data
}
</script>
```

#### （4）Detail.vue编写页面显示

```vue
......
<span>发布于 {{articleInfo.createTime}}</span>
......
</div>
<h1 class="my-3 text-2xl font-medium dark:text-slate-50">{{articleInfo.title}}</h1>
<hr>
<h2 v-html="articleInfo.content"></h2>
<hr>
```



### 10.4 用户登录

#### （1）Login.vue编写调用

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
import cookie from 'js-cookie'
import { login } from '../api/user';

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

const loginUser = async () => {
    const valid = await userForm.value.validate()
    if(valid) {
        const {data} = await login(user.value) 
        setCookies(data.name, data.uid)
        router.push("/dashboard/article")
    } 
}

const setCookies = (name, uid) => {
    cookie.set('uid', uid, { path: '/' })
    cookie.set('name', name, { path: '/' })
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

#### （2）编写Dashboard.vue页面

```vue
<template>
    <div class="main-panel">
        <div class="menus">
            当前用户：
            <n-button strong secondary type="primary">
                {{name}}
            </n-button>
            
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
    if(!name.value) {
        router.push("/login")
    }
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



### 10.5 文章管理

#### （1）编写Article.vue页面

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
import { listArticle , addArticle , getArticle , updateArticle , deleteArticle } from '../../api/article';
import { ElMessage, ElMessageBox } from 'element-plus'
import RichTextEditor from '../../components/RichTextEditor.vue'
import { listCategory , addCategory , getCategory , updateCategory , deleteCategory } from '../../api/category';

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

// 页面加载完毕以后请求后端接口获取数据
onMounted(() => {
    fetchData()
    getCategortyList()
})

// 远程调用后端分页查询接口
const fetchData = async () => {
    const {data , code , message } = await listArticle(article.value) ;
    list.value = data;
}

//查询所有分类
const getCategortyList = async () => {
    const { data  } = await listCategory()
    categoryList.value = data
}

//进入添加
const addShow = () => {
    article.value = {}
  dialogVisible.value = true
  
}

// 添加
const submit = async () => {
    if(!article.value.id) {
        const { code } = await addArticle(article.value) ;
        article.value.cid=null
        dialogVisible.value = false
        ElMessage.success('操作成功')
        fetchData()
    }else {
        console.log(article.value)
        const { code } = await updateArticle(article.value) ;
        dialogVisible.value = false
        ElMessage.success('操作成功')
        fetchData()
    }
}

// 修改按钮点击事件处理函数
const show = (row) => {
    getDetail(row.id)
    dialogShowVisible.value = true
}

const getDetail = async (id) => {
    const {data , code , message } = await getArticle(id) ;
    article.value = data;
}

// 删除数据
const deleteById = (row) => {
    ElMessageBox.confirm('此操作将永久删除该记录, 是否继续?', 'Warning', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
    }).then(async () => {
        const {code } = await deleteArticle(row.id)
        ElMessage.success('删除成功')
        fetchData()
    })
}
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



### 10.6 分类管理

#### （1）编写Category.vue页面

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
import { listCategory , listCategoryPage, addCategory , getCategory , updateCategory , deleteCategory } from '../../api/category';
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

// 页面加载完毕以后请求后端接口获取数据
onMounted(() => {
    fetchData() ;
})

// 搜索按钮点击事件处理函数
const searchCategory = () => {
    fetchData()
}

const resetData = () => {
    queryDto.value.cname = ""
    fetchData()
}

// 远程调用后端分页查询接口
const fetchData = async () => {
    const {data , code , message } = await listCategoryPage(pageParams.value.page , pageParams.value.limit , queryDto.value) ;
    list.value = data.list;
    total.value = data.totalCount
}

//进入添加
const addShow = () => {
  dialogVisible.value = true
  category.value = {}
}

// 添加
const submit = async () => {
    if(!category.value.cid) {
        const { code } = await addCategory(category.value) ;
        alert(code)
        dialogVisible.value = false
        ElMessage.success('操作成功')
        pageParams.value.page = 1
        fetchData()
    }else {
        console.log(category.value)
        const { code } = await updateCategory(category.value) ;
        dialogVisible.value = false
        ElMessage.success('操作成功')
        pageParams.value.page = 1
        fetchData()
    }
}

// 修改按钮点击事件处理函数
const editShow = (row) => {
    category.value = row
    dialogVisible.value = true
}

// 删除数据
const deleteById = (row) => {
    ElMessageBox.confirm('此操作将永久删除该记录, 是否继续?', 'Warning', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
    }).then(async () => {
        const {code } = await deleteCategory(row.cid)
        ElMessage.success('删除成功')
        pageParams.value.page = 1
        fetchData()
    })
}
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



### 10.7 用户中心

#### （1）编写User.vue页面

```vue
<template>
    <div class="login-panel">
        <el-card title="用户中心" class="bg-panel">
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
        </el-card>
    </div>
</template>

<script setup>
import { ref, reactive, inject, onMounted, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { getUser,updateUser } from '../../api/user';
import { ElMessage, ElMessageBox } from 'element-plus'
import cookie from 'js-cookie'

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

onMounted(() => {
    loadUser()
})


const loadUser = async () => {
    let uid= cookie.get('uid')
    const { data } = await getUser(uid)
    userInfo.value = data
}

const modifyUser = async () => {
    const { data } = await updateUser(userInfo.value)
    ElMessage.success('操作成功')
}
</script>

<style lang="scss" scoped>
.login-panel {
    width: 500px;
    margin: 0;
}
</style>
```

