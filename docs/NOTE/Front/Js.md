---

hideFooter: true

---
# JavaScript面试题总结 #

::: tip 契子
- https://github.com/abc-club/free-resources/blob/master/INTERVIEW.md
- https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers
- https://yuchengkai.cn/docs/frontend/#typeof
- https://juejin.im/book/5bdc715fe51d454e755f75ef
:::

## &01.题一--变量提升 ##

- var和let的区别，变量提升

var存在变量提升

```bash
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();   //undefined, ReferenceError
```
- let 存在变量提升，只是存在一个“暂时死区”，在变量初始化或者未赋值前不被允许访问

```bash
let name = 'ConardLi'
{
  console.log(name) // Uncaught ReferenceError: name is not defined
  let name = 'code秘密花园'
}
```

## &02.题二--块级作用域 ##

- let具备块级作用域，var全局作用域；异步执行和同步执行

```bash
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);  //3 3 3
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);   //0 1 2
}
```

## &03.题三--ES6箭头函数的作用域 ##

- 箭头函数的作用域（this为父作用域的this，不是调用时的this）
箭头函数的作用域永远指向其父作用域，任何方法都改变不了，普通函数的this指向调用它的那个对象

- 箭头函数不能成为构造函数，不能使用new

```bash
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

shape.diameter();    //20
shape.perimeter();   //NaN
```

## &04.题四-- ##

- 一元加号会尝试把boolean类型转换为数字，true转换为1，false转换为0
```bash
+true;         // 1
!"Lydia";      // 0
```

## &05.题五--对象引用 ##

- 在JavaScript中，当使用等号使对象互相相等时，其实是引用同一个原型对象，此时更改一个对象的值，可以改变所有对象的值
- JS 六大基础类型：Boolean、String、Number、Undefined、Null、Object（复杂数据类型）

```bash
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);   //hello
```

## &06.题六--对象类型判断 ##

- == 会引发隐式类型转换
- new Number是一个内置的构造函数：有一堆额外的功能，是一个对象

```bash
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);     //true
console.log(a === b);    //false
console.log(b === c);    //false
```

## &07.题七--static静态方法 ##

- JavaScript的静态方法仅在创建它们的构造函数中存在，并且不能传递给任何他们的子级

```bash
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
freddie.colorChange("orange");   //TypeError
```

## &08.题八-- ##

- 可以使用“use strict”。 这可以确保在将变量赋值之前必须声明变量。

## &09.题九--原型链添加方法 ##

- 如需要额外给暴露的对象添加方法，可以在对象的原型链上添加

```bash
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = () => this.firstName + this.lastName;

console.log(member.getFullName());    //TypeError

Person.prototype.getFullName = XXX
```
## &10.题十--不使用new关键字 ##

- 当使用new时，指的是创建的新空对象；不使用new时，指的是全局对象

```bash
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);          //Person {firstName: "Lydia", lastName: "Hallie"}
console.log(sarah);          // undefined

```

## &11.题十一--基础对象 ##

- 除基础对象外，所有对象都有原型。

## &12.题十二--自增运算 ##

- 后缀一元运算：先返回，再增加
- 前缀一元运算：先增加，再返回

```bash
let number = 0;
console.log(number++);     //0
console.log(++number);     //2
console.log(number);       //2
```
## &13.题十三--对象比较 ##

- 在比较相等性的时候，原始类型通过他们的值进行对比，而对象通过他们的引用进行比较。

```bash
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("You are an adult!");
  } else if (data == { age: 18 }) {
    console.log("You are still an adult.");
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });    //Hmm.. You don't have an age I guess
// 我们作为参数传递的对象和我们用于检查相等性的对象在内存中位于不同位置，所以它们的引用是不同的。
```

## &14.题十四- ##
```bash
var arr = [1,2,4];
console.log(typeof arr);    //object
```

## &15.深拷贝与浅拷贝的区别- ##

- 深拷贝复制变量，对于非基本类型的变量，则递归至基本类型变量后，再复制；深拷贝后的对象与原来的对象是完全隔离的，互不影响的，对
一个对象的修改不会影响到另一个对象

- 浅拷贝是会将对象的每个属性进行依次复制，但是当对象的属性值是引用类型时，实质复制的是其引用，当引用指向的值改变时也会跟着变化。

- Object.assign是浅拷贝，只是对第一层属性进行了深拷贝

```bash
let s ={name: {asd: '123'}}
let d = Object.assign({}, s)
d.name.asd = '123456789'
console.log(d, s)     //d：{name: {asd: '123456789'}}    s：{name: {asd: '123456789'}}

let o ={name: {asd: '123'}}
let p = Object.assign({}, o)
p.name = '123456789'
console.log(p, o)      //p：{name: 123456789}    o: {name: {asd: '123'}}

```

- 深拷贝实现

1.最简单的方法： JSON.parse(JSON.stringify(obj))

```bash
deepClone(obj, hash = new WeakMap) {
        if(obj instanceof RegExp) {
          return new RegExp(obj);
        }
        if(obj instanceof Date) {
          return new Date(obj);
        }
        if(obj === null || typeof obj !== 'object') {
          return obj;
        }

        if(hash.has(obj)) {
          return hash.get(obj);
        }
        //如果obj是数组，则返回[function： Array]
        //如果obj是对象，则返回Object[function: Object]
        let t = new obj.constructor();
        hash.set(obj, t);
        for(let key in obj) {
          if(obj.hasOwnPropery(key)) {
            t[key] = deepClone(obj[key], hash)
          }
        }
        return t;

      },
```

- WeakMap对象保存键值对的时候，与Map不同的是其键值必须是对象，因为键是弱引用，在键对象消失后自动释放内存

好处：1.element中使用点击或监听，可防止内存泄漏

    2.可用来部署类中的私有属性


## &16.js this的指向- ##

- 默认绑定：不加任何修饰符直接调用函数

```bash
function foo() {
  console.log(this.a)   // 输出 a
}

var a = 2;  //  变量声明到全局对象中

foo();
```
- 显示绑定：函数创建时，this的对象指向调用它的对象

```bash
function foo() {
  console.log(this.a);
}

let obj1 = {
  a: 1,
  foo,
};

let obj2 = {
  a: 2,
  foo,
}

obj1.foo();   // 输出 1
obj2.foo();   // 输出 2
```
- 显示绑定：使用call()、apply()、bind()中改变this的指向，call和apply是立即执行函数

```bash
function foo() {
  console.log(this.a);  // 输出 1
  bar.apply({a: 2}, arguments);
}

function bar(b) {
  console.log(this.a + b);  // 输出 5
}

var a = 1;
foo(3);
```

- new关键字可以改变this的指向：创新一个新的对象，执行prototype连接，改变this的调用

```bash
function Foo(){
    this.x = 10;
    console.log(this);    //Foo {x:10}
}
var foo = new Foo();
console.log(foo.x);      //10

```
```bash
function Foo(){
    this.x = 10;
    console.log(this);    //Window
}
var foo = Foo();
console.log(foo.x);      //undefined

```
- 坑一：对象方法相关

```bash
var obj = {
    x: 10,
    foo: function () {
        console.log(this);        //Object
        console.log(this.x);      //10
    }
};
obj.foo();
```

```bash
var obj = {
    a: 1,
    foo: function() {
      function fo() {
        console.log(this);    //window
        console.log(this.a);  //undefined
      }
      fo();
    }
  }
  obj.foo();
```
说明：fo虽然是在obj里面调用，但是最终执行时，还是一个普通的函数，this指向window

- 箭头函数对this的修复--始终指向外层调用者

```bash
var obj = {
    x: 10,
    foo: function() {
        var fn = () => {
            return () => {
                return () => {
                    console.log(this);      //Object {x: 10}
                    console.log(this.x);    //10
                }
            }
        }
        fn()()();
    }
}
obj.foo();
```

参考文章：
https://www.cnblogs.com/pssp/p/5216085.html

https://zhuanlan.zhihu.com/p/25294187

## &17.js 闭包##

- 作用一：读取函数内部的变量（作用域链）

- 作用二：可以让这些变量始终保持在内存中

## &18.js 箭头函数和普通函数的区别 ##

1. 箭头函数是匿名函数，不能作为构造函数，不能使用new

2. 箭头函数不绑定arguments，取而代之的是rest参数...解决

```bash
function A(a){
  console.log(arguments);
}
A(1,2,3,4,5,8);  //  [1, 2, 3, 4, 5, 8, callee: ƒ, Symbol(Symbol.iterator): ƒ]


let B = (b)=>{
  console.log(arguments);
}
B(2,92,32,32);   // Uncaught ReferenceError: arguments is not defined


let C = (...c) => {
  console.log(c);
}
C(3,82,32,11323);  // [3, 82, 32, 11323]
```

3. 箭头函数不绑定this，会捕获其所在上下文的this值，作为自己的this值

```bash
var obj = {
  a: 10,
  b: () => {
    console.log(this.a); // undefined
    console.log(this); // Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
  },
  c: function() {
    console.log(this.a); // 10
    console.log(this); // {a: 10, b: ƒ, c: ƒ}
  }
}
obj.b(); 
obj.c();

```

```bash
var obj = {
  a: 10,
  b: function(){
    console.log(this.a); //10
  },
  c: function() {
     return ()=>{
           console.log(this.a); //10
     }
  }
}
obj.b(); 
obj.c()();

```


## &18.js  ##

- 打印对象的时候会自动调用toString方法

## &19.css-- 替换元素  ##

- 替换元素就是根据浏览器元素的标签和属性，来决定元素的具体显示内容，在其显示中生成了框，例如：
```bash
<img>、<input>、<textarea>、<select>、<object>

```

## &20.浏览器 --常规 ##

- https://juejin.im/post/5df5bcea6fb9a016091def69

## &21.js --原型链 ##

demo1.

```bash
function People(name, age) {
    this.name = name;
    this.age = age;
    this.eat = function() {
      console.log(this.name + 'iseating');
    }
  }
  let p1 = new People('baozi', '1');
  let p2 = new People('baozi2', '2');
  console.log(p1.eat == p2.eat);    //false

```
new 关键词会创建不同的对象，eat函数对比为false表示，p1和p2被分配在堆内存中，如果创建很多，会导致内存不够使用

```bash
function People(name, age) {
    this.name = name;
    this.age = age;
    this.eat = function() {
      console.log(this.name + 'iseating');
    }
  }
  People.prototype.eat = function() {
    console.log(this.name + 'eating');
  }
  let p1 = new People('baozi', 1);
  let p2 = new People('baozi', 2);
  console.log(p1.eat == p2.eat);   //true

```
eat函数指向堆中同一个地址

- prototype原型；__proto__原型链；Object.prototype.__proto__ == null

```bash
Object.__proto__ === Function.prototype;
Function.prototype.__proto__ === Object.prototype;
Object.prototype.__proto__ === null;
```

## &22.js --event loop代码的执行顺序 ##

event loop都不陌生，是指主线程从“任务队列”中循环读取任务

```bash
setTimeout(function () {
  console.log(3);
}, 0);

Promise.resolve().then(function () {
  console.log(2);
});
console.log(1);

//1,2,4
```

以上：js的执行顺序是同步任务在主线程中优先执行；异步执行代码顺序如下：script(主程序代码)—>process.nextTick—>Promises...——>setTimeout——>setInterval——>setImmediate——> I/O——>UI rendering

```bash
setTimeout(function(){console.log(1)},0);
new Promise(function(resolve,reject){
   console.log(2);
   resolve();
}).then(function(){console.log(3)
}).then(function(){console.log(4)});

process.nextTick(function(){console.log(5)});

console.log(6);
//输出2,6,5,3,4,1
```

## &23.js函数优先级 ## 
- 函数表达式比匿名函数优先级高；变量提升，函数提升

https://www.cnblogs.com/xxcanghai/p/4991870.html

https://www.cnblogs.com/xxcanghai/p/4991870.html

https://juejin.im/post/5df50a60518825121f699c4d

```bash
 function getName() {
    console.log('2');
  }
  var getName = function() {
    console.log('1');
  }
  getName();  //1
```

## &24.js深入浅出之--词法作用域和动态作用域 ## 
- js采用词法作用域，函数的作用域在函数定义的时候确定
- 词法作用域相对的就是动态作用域，函数的作用域就是在函数调用的时候确定
- 在进入执行上下文时，首先会处理函数声明，其次会处理变量声明，如果如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性。

```bash
 var value = 1;

function foo() {
    console.log(value);
}

function bar() {
    var value = 2;
    foo();
}

bar();    //1
```

```bash
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();  //local scope
```

以上，因为js是词法作用域，在声明的时候作用域已经绑定，因此打印出声明时的值。

```bash
console.log(foo);  //打印出函数体

function foo(){
    console.log("foo");
}

var foo = 1;    
```

以上，函数声明优先级比变量声明高，不会被覆盖

## &25.js原型与原型链 ##

- prototype
- __proto__
- new
- call()/apply()/bind()
- this

问题1：了解this，说说this的作用

答：this是函数上下文被创建时确定的，即this的指向其实是函数被调用时确定的。同一个函数，由于调用方式的不同，可能this的对象不同。在函数执行过程中，this一旦被确定，就不会更改。
















