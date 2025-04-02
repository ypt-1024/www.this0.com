## 二、ECMA6Script

### 2.1. es6的介绍

> ECMAScript 6，简称ES6，是**JavaScript**语言的一次重大更新。它于**2015**年发布，是原来的ECMAScript标准的第六个版本，大大提升了JavaScript的开发体验。

### 2.2 es6的变量和模板字符串

> ES6 新增了`let`和`const`，用来声明变量,使用的细节上也存在诸多差异

+ let 和var的差别

  1、let有块级作用域，非函数的花括号遇见let会有块级作用域，也就是只能在花括号里面访问。

  2、let 不能重复声明

  3、let不会预解析进行变量提升（先声明，在使用）

  4、let 定义的全局变量不会作为window的属性

  5、let在es6中推荐优先使用

```html
<script>
    //1. let只有在当前代码块有效代码块. 代码块、函数、全局
    {
      let a = 1
      var b = 2
    }   
    console.log(a);  // a is not defined   花括号外面无法访问
    console.log(b);  // 可以正常输出

    //2. 不能重复声明
    let name = '天真'
    let name = '无邪'

    //3. 不存在变量提升（先声明，在使用）
    console.log(test) //可以     但是值为undefined
    var test = 'test'
    console.log(test1) //不可以  let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。
    let test1 = 'test1' 


    //4、不会成为window的属性   
    var a = 100
    console.log(window.a) //100
    let b = 200
    console.log(window.b) //undefined

</script>
```

+ const和var的差异

  1、新增const和let类似，只是const定义的变量不能修改

  2、并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。

```html
<script>
 
    //1.常量声明必须有初始化值
    //const A ; //报错

    //2.常量值不可以改动
    //const A  = 'this0'
    //A  = 'xx' //不可改动

    //3.和let一样，块儿作用域
    {
        const A = 'atguigu'
        console.log(A);
    }
    //console.log(A);

    //4.对应数组和对象元素修改，不算常量修改，修改值，不修改地址
    const TEAM = ['刘德华','张学友','郭富城'];
    TEAM.push('黎明');
    TEAM=[] // 报错
    console.log(TEAM)
</script>
```

> 模板字符串（template string）是增强版的字符串，用反引号（`）标识  

1、字符串中可以出现换行符

2、可以使用 ${xxx} 形式输出变量和拼接变量

``` html
<script>
 
    // 1 多行模板字符串
    let ulStr2 = `
        <ul>
        	<li>JAVA</li>
        	<li>html</li>
        	<li>VUE</li>
        </ul>`
    console.log(ulStr2)        

    // 2 模板字符串拼接
    let infoStr2 =`${name}被评为本年级优秀学员`
    console.log(infoStr2)
</script>
```



### 2.3 es6的解构表达式

> ES6 的解构赋值是一种方便的语法，可以快速将数组或对象中的值拆分并赋值给变量。解构赋值的语法使用花括号 `{}` 表示对象，方括号 `[]` 表示数组。通过解构赋值，函数更方便进行参数接受等！

> **数组解构赋值**

+ 可以通过数组解构将数组中的值赋值给变量，语法为：

```javascript
let [a, b, c] = [1, 2, 3]; //新增变量名任意合法即可，本质是按照顺序进行初始化变量的值
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

+ 该语句将数组 \[1, 2, 3] 中的第一个值赋值给 a 变量，第二个值赋值给 b 变量，第三个值赋值给 c 变量。
  可以使用默认值为变量提供备选值，在数组中缺失对应位置的值时使用该默认值。例如：

```javascript
let [a, b, c, d = 4] = [1, 2, 3];
console.log(d); // 4
```

> **对象解构赋值**

+ 可以通过对象解构将对象中的值赋值给变量，语法为：

```javascript
let {a, b} = {a: 1, b: 2};
//新增变量名必须和属性名相同，本质是初始化变量的值为对象中同名属性的值
//等价于 let a = 对象.a  let b = 对象.b
  
console.log(a); // 1
console.log(b); // 2
```

+ 该语句将对象 {a: 1, b: 2} 中的 a 属性值赋值给 a 变量，b 属性值赋值给 b 变量。
  可以为标识符分配不同的变量名称，使用 : 操作符指定新的变量名。例如：

```javascript
let {a: x, b: y} = {a: 1, b: 2};
console.log(x); // 1
console.log(y); // 2
```

> **函数参数解构赋值**

+ 解构赋值也可以用于函数参数。例如：

```javascript
function add([x, y]) {
  return x + y;
}
add([1, 2]); // 3
```

+ 该函数接受一个数组作为参数，将其中的第一个值赋给 x，第二个值赋给 y，然后返回它们的和。

+ ES6 解构赋值让变量的初始化更加简单和便捷。通过解构赋值，我们可以访问到对象中的属性，并将其赋值给对应的变量，从而提高代码的可读性和可维护性。

### 2.4 es6的箭头函数

> ES6 允许使用“箭头” 义函数。语法类似Java中的Lambda表达式

#### 2.4.1 声明和特点

```html
<script>

    //ES6 允许使用“箭头”（=>）定义函数。
    //1. 函数声明
    let fn1 = function(){}
    let fn2 = ()=>{} //箭头函数,此处不需要书写function关键字
    let fn3 = x =>{} //单参数可以省略(),多参数无参数不可以!
    let fn4 = x => console.log(x) //只有一行方法体可以省略{};
    let fun5 = x => x + 1 //当函数体只有一句返回值时，可以省略花括号和 return 语句
    
    //2. 使用特点 箭头函数this关键字
    // 在 JavaScript 中，this 关键字通常用来引用函数所在的对象，
    // 或者在函数本身作为构造函数时，来引用新对象的实例。
    // 并且是由箭头函数定义时的上下文来决定的，而不是由函数调用时的上下文来决定的。
    // 箭头函数没有自己的this，this指向的是外层上下文环境的this
    
    let person ={
        name:"张三",
        showName:function (){
            console.log(this) //  这里的this是person
            console.log(this.name)
        },
        viewName: () =>{
            console.log(this) //  这里的this是window
            console.log(this.name)
        }
    }
    person.showName()
    person.viewName()
 
    //this应用
    function Counter() {
        this.count = 0;
        setInterval(() => {
            // 这里的 this 是上一层作用域中的 this，即 Counter实例化对象
            this.count++;
            console.log(this.count);
        }, 1000);
    }
    let counter = new Counter();

</script>
```

### 2.6 es6的promise异步处理

#### 2.6.1 普通函数和回调函数

> 普通函数: 正常调用的函数,一般函数执行完毕后才会继续执行下一行代码

``` html
<script>
    let fun1 = () =>{
        console.log("fun1 invoked")
    }
    // 调用函数
    fun1()
    // 函数执行完毕,继续执行后续代码
    console.log("other code processon")
</script>
```

> 回调函数: 一些特殊的函数,表示未来才会执行的一些功能,后续代码不会等待该函数执行完毕就开始执行了

```html
<script>
    // 设置一个2000毫秒后会执行一次的定时任务
    setTimeout(function (){
        console.log("setTimeout invoked")
    },2000)
    console.log("other code processon")
</script>
```

#### 2.6.2 Promise 简介

> 前端中的异步编程技术，类似Java中的多线程+线程结果回调！

+ Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了`Promise`对象。（传统解决方案->Promise的历程）。

+ 所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。

> `Promise`对象有以下两个特点。

​	（1）Promise对象代表一个异步操作，有三种状态：`Pending`（进行中）、`Resolved`（已完成，又称 Fulfilled）和`Rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

​	（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从`Pending`变为`Resolved`和从`Pending`变为`Rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。

#### 2.6.3 Promise 基本用法

> ES6规定，Promise对象是一个构造函数，用来生成Promise实例。

```html
<script>

/*
 1.实例化promise对象
 参数: resolve,reject随意命名
 */
let promise = new Promise(function(resolve,reject){

  console.log("promise do some code ... ...")

  // 参数: resolve,reject分别负责处理 成功 和 失败 两个函数!
  //调用resolve就变成成功状态，调用reject就变成失败状态
      //resolve("promise success")
  reject("promise fail")
})

//2.获取回调函数结果  then在这里会等待promise中的运行结果,但是不会阻塞代码继续运行
promise.then(

    //前者是成功的回调，里面的参数value也可以自己命名，自动获取resolve里面的参数
    function(value){console.log(`promise中执行了resolve:`+value)},

    //后者是失败的回调，同理
    function(error){console.log(`promise中执行了reject:`+error)}
)
//这个返回也是一个promise函数

// 3 其他代码执行
console.log('other code2222 invoked')
</script>
```

#### 2.6.4 Promise catch()

> `Promise.prototype.catch`方法是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。

```html
<script>
    let promise =new Promise(function(resolve,reject){
        console.log("promise do some code ... ...")
        // 故意响应一个异常对象
        throw new Error("error message")
    })
    console.log('other code1111 invoked')
    
        */
    
    //简言之，可以简写，替换成这个样子：then里面放成功的函数，catch放失败的函数
    promise.then(
        function(resolveValue){console.log(`promise中执行了resolve:${resolveValue}`)}
    ).catch(
        function(error){console.log(error)} 
    )
    console.log('other code2222 invoked')
</script>
```

#### 2.6.5 Promise.all()

> Promise.all()可以帮助我们比较简单的获得多个promise对象回调函数中返回的结果

``` html
<script>
    let p1=new  Promise((resovle,reject )=>{
        resovle("OK")
    })
    let p2 = Promise.resolve("success")
    let p3 = new  Promise((resovle,reject )=>{
        //reject("yeah")
        throw new Error("oh no")
    })
    // 通过Promise.all方法,获取多个promise对象的结果
    // 返回值仍然是一个promise对象
    let promiseAll =Promise.all([p1,p2,p3])
    console.log(promiseAll)
    // 如果所有的promise都resolve了,走then
    // 如果有一个promise走了reject() 或者抛出了异常,走catch
    promiseAll.then(results =>{
        console.log(results)
    }).catch( error=>{
        console.log(error)
    })
</script>
```

​		then中的reject()的对应方法可以在产生异常时执行,接收到的就是异常中的提示信息
​		`then中可以只留一个resolve()的对应方法,reject()方法可以用后续的catch替换`
​		then中的reject对应的回调函数被后续的catch替换后,catch中接收的数据是一个异常

####  2.6.6 async和await的使用

> &#x20;async和await是ES6中用于处理异步操作的新特性。通常，异步操作会涉及到Promise对象，而async/await则是在Promise基础上提供了更加直观和易于使用的语法。

1 async标识函数后,async函数的返回值会变成一个promise对象

2 如果函数内部返回的是一个`promise对象`,则`async函数返回的状态与结果由该对象接手`

``` html
<script>
    	async function fun1(){
            
            //2.如果方法正常返回结果,async函数的状态就是resolved ，return后的结果就是成功状态的返回值
            //return 10
            
            //模拟出现异常
           // 3 如果函数内部抛出的是一个`异常`,则 async函数返回的promise就是一个失败状态
            //throw new Error("something wrong")
            
            //4. 如果函数返回的是一个romise,则状态由内部promise决定
            
            let promise = Promise.reject("heihei")
            return promise
        }

        let promise =fun1()

        promise.then(
            function(value){
                console.log("success:"+value)
            }
        ).catch(
            function(value){
                console.log("fail:"+value)
            }
        )
</script>
```

> await

1. await右侧的表达式一般为一个promise对象,但是也可以是一个其他值，如果表达式是其他值,则直接返回该值，当然，是普通值没有意义。
2. `如果表达式是promise对象,await返回的是promise成功的值，一般就是用在这里`
3. await会等右边的promise对象执行结束,然后再获取结果,这个await所在函数里面的后续代码也会等待await的执行
4. await必须在async函数中,但是async函数中可以没有await
5. 如果await右边的promise失败了,就会抛出异常,需要通过 try ... catch捕获处理

``` html
<script>

		async function fun1(){
            return 10
        
        }

        async function fun2(){
            try{
                
                let res = await fun1()
                //let res = await Promise.reject("something wrong")
            }catch(e){
                console.log("catch got:"+e)   
            }
            
            console.log("await got:"+res)
        }

        fun2()
</script>
```

### 2.7 es6的模块化处理

#### 2.7.1模块化介绍

> 模块化是一种组织和管理前端代码的方式，将代码拆分成小的模块单元，使得代码更易于维护、扩展和复用。它包括了定义、导出、导入以及管理模块的方法和规范。

+ ES6模块化的几种暴露和导入方式
  1. 分别导出
  2. 统一导出
  3. 默认导出
+ `ES6中无论以何种方式导出,导出的都是一个对象,导出的内容都可以理解为是向这个对象中添加属性或者方法`

#### 2.7.2 分别导出

![1684461046181](https://blog-resources.this0.com/image/202503260617433.png?x-oss-process=style/this0-blog)

+ module.js 向外分别暴露成员

``` javascript
//1.分别暴露
// 模块想对外导出,添加export关键字即可!
// 导出一个变量
export const PI = 3.14
// 导出一个函数
export function sum(a, b) {
  return a + b;
}
// 导出一个类
export class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(`Hello, my name is ${this.name}, I'm ${this.age} years old.`);
  }
}
```

+ app.js 导入module.js中的成员

``` javascript
/* 
    *代表module.js中的所有成员
    m1代表所有成员所属的对象
*/
import * as m1 from './module.js'
// 使用暴露的属性
console.log(m1.PI)
// 调用暴露的方法
let result =m1.sum(10,20)
console.log(result)
// 使用暴露的Person类
let person =new m1.Person('张三',10)
person.sayHello()
```

+ index.html作为程序启动的入口  导入 app.js  

``` html
<!-- 导入JS文件 添加type='module' 属性,否则不支持ES6的模块化 -->
<script src="./app.js" type="module" /> 
```

#### 2.7.3 统一导出

![1684461701620](https://blog-resources.this0.com/image/202503260617421.png?x-oss-process=style/this0-blog)

+ module.js向外统一导出成员

``` javascript
//2.统一暴露
// 模块想对外导出,export统一暴露想暴露的内容!
// 定义一个常量
const PI = 3.14
// 定义一个函数
function sum(a, b) {
  return a + b;
}
// 定义一个类
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(`Hello, my name is ${this.name}, I'm ${this.age} years old.`);
  }
}
// 统一对外导出(暴露)
export {
	PI,
    sum,
    Person
}
```

+ app.js导入module.js中的成员

``` javascript
/* 
    {}中导入要使用的来自于module.js中的成员
    {}中导入的名称要和module.js中导出的一致,也可以在此处起别名
    {}中如果定义了别名,那么在当前模块中就只能使用别名
    {}中导入成员的顺序可以不是暴露的顺序
    一个模块中可以同时有多个import
    多个import可以导入多个不同的模块,也可以是同一个模块
*/
//import {PI ,Person ,sum }  from './module.js'
//import {PI as pi,Person as People,sum as add}  from './module.js'
import {PI ,Person ,sum,PI as pi,Person as People,sum as add}  from './module.js'
// 使用暴露的属性
console.log(PI)
console.log(pi)
// 调用暴露的方法
let result1 =sum(10,20)
console.log(result1)
let result2 =add(10,20)
console.log(result2)
// 使用暴露的Person类
let person1 =new Person('张三',10)
person1.sayHello()
let person2 =new People('李四',11)
person2.sayHello()
```

#### 2.7.4 默认导出

![1684463528680](https://blog-resources.this0.com/image/202503260617509.png?x-oss-process=style/this0-blog)



+ modules混合向外导出

``` javascript
// 3默认和混合暴露
/* 
    默认暴露语法  export default sum
    默认暴露相当于是在暴露的对象中增加了一个名字为default的属性
    三种暴露方式可以在一个module中混合使用

*/
export const PI = 3.14
// 导出一个函数
function sum(a, b) {
  return a + b;
}
// 导出一个类
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(`Hello, my name is ${this.name}, I'm ${this.age} years old.`);
  }
}

// 导出默认
export default sum
// 统一导出
export {
   Person
}

```

+ app.js 的default和其他导入写法混用

``` javascript
/* 
    *代表module.js中的所有成员
    m1代表所有成员所属的对象
*/
import * as m1 from './module.js'
import {default as add} from './module.js' // 用的少
import add2 from './module.js' // 等效于 import {default as add2} from './module.js'

// 调用暴露的方法
let result =m1.default(10,20)
console.log(result)
let result2 =add(10,20)
console.log(result2)
let result3 =add2(10,20)
console.log(result3)

// 引入其他方式暴露的内容
import {PI,Person} from './module.js'
// 使用暴露的Person类
let person =new Person('张三',10)
person.sayHello()
// 使用暴露的属性
console.log(PI)
```
