---
abbrlink: '0'
---
### ES6

#### 介绍

ES6带来了大量的新特性，包括箭头函数、模板字符串、let和const关键字、解构、默认参数值、模块系统等等，大大提升了JavaScript的开发体验。`由于VUE3中大量使用了ES6的语法,所以ES6成为了学习VUE3的门槛之一`

#### 变量

##### let 和var的差别

1、`let 不能重复声明`

2、let有块级作用域，非函数的花括号遇见let会有块级作用域，也就是只能在花括号里面访问。

3、let 定义的全局变量不会作为window的属性

##### const和var的差异

const和let类似，只是const定义的变量不能修改

//4.对应数组和对象元素修改，不算常量修改，修改值，不修改地址

```js
    const TEAM = ['刘德华','张学友','郭富城'];
    TEAM.push('黎明');
    TEAM=[] // 报错
```

#### 模板字符串

##### 1、字符串中可以出现换行符

##### 2、可以使用 ${xxx} 形式输出变量和拼接变量

#### 解构表达式

##### 数组解构赋值

```js
let [a, b, c] = [1, 2, 3]; //新增变量名任意合法即可，本质是按照顺序进行初始化变量的值
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

可以使用默认值为变量提供备选值

```javascript
let [a, b, c, d = 4] = [1, 2, 3];
console.log(d); // 4
```

##### 对象解构赋值

```js
let {a, b} = {a: 1, b: 2};
//新增变量名必须和属性名相同，本质是初始化变量的值为对象中同名属性的值
//等价于 let a = 对象.a  let b = 对象.b
  
console.log(a); // 1
console.log(b); // 2
```

该语句将对象 {a: 1, b: 2} 中的 a 属性值赋值给 a 变量，b 属性值赋值给 b 变量。
可以为标识符分配不同的变量名称，使用 : 操作符指定新的变量名。例如：

```javascript
let {a: x, b: y} = {a: 1, b: 2};
console.log(x); // 1
console.log(y); // 2
```

##### 函数参数解构赋值

+ 解构赋值也可以用于函数参数。例如：

```javascript
function add([x, y]) {
  return x + y;
}
add([1, 2]); // 3
```

该函数接受一个数组作为参数，将其中的第一个值赋给 x，第二个值赋给 y，然后返回它们的和。

#### rest和spread

rest参数,在形参上使用 和JAVA中的可变参数几乎一样

```js
<script>
    // 1 参数列表中多个普通参数  普通函数和箭头函数中都支持
    let fun1 = function (a,b,c,d=10){console.log(a,b,c,d)}
    fun1(1,2,3)

    // 2 ...作为参数列表,称之为rest参数 普通函数和箭头函数中都支持 ,因为箭头函数中无法使用arguments,rest是一种解决方案
    let fun3 = function (...args){console.log(args)}
    let fun4 = (...args) =>{console.log(args)}
    fun3(1,2,3)
    fun4(1,2,3,4)
    // rest参数在一个参数列表中的最后一个只,这也就无形之中要求一个参数列表中只能有一个rest参数
    //let fun5 =  (...args,...args2) =>{} // 这里报错
</script>
```

spread参数,在实参上使用rest

```js
<script>
    let arr =[1,2,3]
    //let arrSpread = ...arr;// 这样不可以,...arr必须在调用方法时作为实参使用

    //应用场景1 合并数组
    let arr2=[4,5,6]
    let arr3=[...arr,...arr2]
    console.log(arr3)
    //应用场景2 合并对象属性
    let p1={name:"张三"}
    let p2={age:10}
    let p3={gender:"boy"}
    let person ={...p1,...p2,...p3}
    console.log(person)

</script>
```

#### 对象的创建和拷贝

ES6中新增了对象创建的语法糖,支持了class extends constructor等关键字,让`ES6的语法和面向对象的语法更加接近`

```js
class Person{
      // 属性
      #n;
      age;
      get name(){
          return this.n;
      }
      set name(n){
          this.n =n;
      }
      // 实例方法
      eat(food){
          console.log(this.age+"岁的"+this.n+"用筷子吃"+food)
      }
      // 静态方法
      static sum(a,b){
          return a+b;
      }
      // 构造器
      constructor(name,age){
          this.n=name;
          this.age = age;

      }
  }
  let person =new Person("张三",10);
  // 访问对象属性
  // 调用对象方法
  console.log(person.name)
  console.log(person.n)
  person.name="小明"
  console.log(person.age)
  person.eat("火锅")
  console.log(Person.sum(1,2))

  class Student extends  Person{
      grade ;
      score ;
      study(){

      }
      constructor(name,age ) {
          super(name,age);
      }
  }

  let stu =new Student("学生小李",18);
  stu.eat("面条")
```

> 对象的拷贝,快速获得一个和已有对象相同的对象的方式

浅拷贝

``` html
<script>
    let person ={
        name:'张三',
        language:arr
    }
    // 浅拷贝,person2和person指向相同的内存
    let person2 = person;
    person2.name="小黑"
    console.log(person.name)
</script>
```

深拷贝

``` html
<script>
    let arr  =['java','c','python']
    let person ={
        name:'张三',
        language:arr
    }
    // 深拷贝,通过JSON和字符串的转换形成一个新的对象
    let person2 = JSON.parse(JSON.stringify(person))
    person2.name="小黑"
    console.log(person.name)
    console.log(person2.name) 
</script>
```

//TODO，深拷贝的字符串转换



#### es6的模块化处理

##### 1 模块化介绍

+ ES6模块化的几种暴露和导入方式
  1. 分别导出
  2. 统一导出
  3. 默认导出
+ `ES6中无论以何种方式导出,导出的都是一个对象,导出的内容都可以理解为是向这个对象中添加属性或者方法`

##### 2 分别导出

![1684461046181](https://blog-resources.this0.com/image/202403301652791.png?x-oss-process=style/this0-blog )

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

##### 3 统一导出

![1684461701620](https://blog-resources.this0.com/image/202403301652784.png?x-oss-process=style/this0-blog )

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

##### 4 默认导出

![1684463528680](https://blog-resources.this0.com/image/202403301652999.png?x-oss-process=style/this0-blog )



+ modules混合向外导出

``` javascript
// 3默认和混合暴露
/* 
    默认暴露语法  export default sum
    默认对外暴露接口，只能有一个，所以只能出现一次，导出的时候不用加大括号{}，只能先定义，后导出
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

//导入default
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

