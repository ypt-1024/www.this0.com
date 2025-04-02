javaweb干货

`async 用于标识函数，标识函数后,函数的返回值会变成一个promise对象`

ES6模块化的几种暴露和导入方式

1. 分别导出

2. 统一导出

    {}中如果定义了别名,那么在当前模块中就只能使用别名

3. 默认导出

默认暴露语法  export default sum
    默认暴露相当于是在暴露的对象中增加了一个名字为default的属性

```
import {default as add} from './module.js' // 用的少
import add2 from './module.js' // 等效于 import {default as add2} from './module.js'
```

ref响应式数据在绑定到html上时不需要.value

Vite+Vue3关于样式的导入方式

es6的插值表达式

handler的值可以是方法事件处理器,也可以是内联事件处理器`

<button @click="count++">incrCount</button> <br>

```
//user = {};// 这中写法会将数据变成非响应的,应该是user.username=""
```