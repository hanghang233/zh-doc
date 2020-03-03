---

hideFooter: true

---
# js--面试总结 #

::: tip 契子
- https://github.com/huyaocode/webKnowledge --- 大神面经，值得参考
- 
:::

## &01.如何解决异步回调地域 ##

## &02.说一下类的创建与继承 ##
答：在js中创建一个函数就是创建一个对象，一个对象就是一个类。每一个创建的函数都有一个原型对象，通过原型对象我们可以实现JS类中的继承。
- 创建对象有几种方法
    1.构造函数模式
    2.new操作符 + Object创建对象
    ```bash
        var person = new Object();
        person.name = "lisi";
        person.age = 21;
        person.family = ["lida","lier","wangwu"];
        person.say = function(){
            alert(this.name);
        }
    ```
    3.工厂模式--instanceof无法判断它是谁的实例，只能判断它是对象
    ```bash
       function createPerson(name,age,family) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.family = family;
        o.say = function(){
            alert(this.name);
        }
        return o;
    }

    var person1 =  createPerson("lisi",21,["lida","lier","wangwu"]);   //instanceof无法判断它是谁的实例，只能判断他是对象，构造函数都可以判断出
    var person2 =  createPerson("wangwu",18,["lida","lier","lisi"]);
    console.log(person1 instanceof Object);                           //true
    ```
    4.构造函数模式
    ```bash
       function Person(name,age,family) {
            this.name = name;
            this.age = age;
            this.family = family;
            this.say = function(){
                alert(this.name);
            }
        }
        var person1 = new Person("lisi",21,["lida","lier","wangwu"]);
        var person2 = new Person("lisi",21,["lida","lier","lisi"]);
        console.log(person1 instanceof Object); //true
        console.log(person1 instanceof Person); //true
        console.log(person2 instanceof Object); //true
        console.log(person2 instanceof Person); //true
        console.log(person1.constructor);      //constructor 属性返回对创建此对象的数组、函数的引用
    ```
    5.原型模式
    6.混合模式
- 原型、构造函数、实例、原型链
![npm-script](./imgs/prototype.png)
    * * prototpye原型：指的是一个对象，实例“继承”那个对象的属性。在原型上定义的属性，通过“继承”，实例也拥有了这个属性。“继承”行为这个操作是在new操作符中进行的。
    * * __proto__：实例通过__proto__访问到原型
- instanceof的原理

instanceof运算符用于判断一个对象的原型链是否存在一个构造函数的prototype属性。

判断实例对象的属性和构造函数的属性是否是同一地址。实例对象.__proto === 构造函数.prototype ,只要是同一原型链的构造函数实例，instanceof都会被看作是true

判断原型对象：实例.__proto__.constructor === 构造函数

- new运算符-背后的原理

    1、创建一个新对象

    2、将构造函数的prototype赋值给新对象的__proto__;

    3、构造函数中的this指向新对象，并调用构造函数

    4、如果构造函数无返回值，或者不是引用类型，返回新对象，否则为构造函数的返回值
    ```bash
    function objectFactory() {
			var obj = {},
			Constructor = [].shift.call(arguments);
			//Constructor是构造函数
			obj.__proto__ = Constructor.prototype;  
			var ret = Constructor.apply(obj, arguments);
			return typeof ret === 'object' ? ret : obj;
		}
    ```


- new Object和Object.create的不同

    1、new Object是用构造函数来创建对象，添加的属性是在自身实例下；Object.create es6创建对象的另外一种方法，可以理解为继承一个对象，添加的对象是在原型上。
    ```bash
       // new Object() 方式创建
        var a = {  rep : 'apple' }
        var b = new Object(a)
        console.log(b) // {rep: "apple"}
        console.log(b.__proto__) // {}
        console.log(b.rep) // {rep: "apple"}

        // Object.create() 方式创建
        var a = { rep: 'apple' }
        var b = Object.create(a)
        console.log(b)  // {}
        console.log(b.__proto__) // {rep: "apple"}
        console.log(b.rep) // {rep: "apple"}
    ```
## &03.面向对象 ##
- 类与实例
    * 类的声明
    * 生成实例
- 类与继承
    * 如何形成继承

    1、构造函数继承
     ```bash
       //构造函数继承--实现部分继承
		function Parent() {
			this.name = "parent";
		}
		//构造函数继承，子类不会继承父类的prototype方法，只能继承构造函数的属性
		Parent.prototype.say = function() {
			console.log('hello');
		}
		function Child() {
			Parent.call(this);   //call改变this指向
			this.type = 'child';
		}
		let child1 = new Child;
		console.log(new Child);
    ```
    2.原型链继承
    ```bash
    //借助原型链实现继承
		//缺点一：新实例无法向构造函数传参
		//缺点二：所有的新实例都会共享父类实例的属性（原型上的属性是共享的，一个实例改变了，另一个也会一起改变）
		function Parent2() {
			this.name = "parent2";
		}
		function Child2() {
			this.type = "child2";
		}
		Child2.prototype = new Parent2;
		console.log(new Child2);
    ```
    3.组合式继承
    ```bash
        //组合方式继承--原型链+构造函数
        //弥补原型链和构造函数的缺点
		function Parent3() {
			this.name = 'parentt3';
		}
		function Child3() {
			Parent3.call(this);
			this.type = 'child3';
		}
		Child3.prototype = Parent3.prototype;//优化Child3.prototype = Object.create(Parent3.prototype);
		var child5 = new Child3();ß
		console.log(child5 instanceof Parent3);  //无法区分child5是谁的实例化
		console.log(child5.constructor);    //指向Parent3
    ```

    * 继承的几种方式

## &04.闭包 ##
- 作用域的特殊情况
1、函数作为参数被传递
    ```bash
        //闭包--ß函数作为返回值
        function create() {
            let a = 100;
            return function() {
                console.log(a)
            }
        }

        const fn = create();
        const a = 200;
        fn();  //100

    ```

2、函数作为返回值被返回--内存不会被释放
```bash
    const a = 100;

	function print(fn) {
		const a = 200;
		fn();
	}
	function fn() {
		console.log(a);
	}
	print(fn)  //100

 ```

总结：闭包自由变量的查找，是在函数定义的地方，向上级作用域查找，不是在调用的地方！！

- 闭包的实际应用场景举例
1、隐藏数据--只提供API--例如定义模块，只提供方法，内部细节隐藏
```bash
    //闭包隐藏数据，只提供API
	function createCache() {
		const data = {};  //闭包中的数据被隐藏，不被外界访问
		return {
			set: function(key, val) {
				data[key] = val;
			},
			get: function(key) {
				return data[key];
			}
		}
	}

	const c = createCache(); //外界无法访问createCache中的data变量
	c.set('a', 100);
	console.log(c,get('a'))  //100

 ```

 2.缓存数据--但是也是缺陷，闭包不会释放内存
```bash
    function f1(){
　　　　var n=999;
　　　　nAdd=function(){n+=1}
　　　　function f2(){
　　　　　　alert(n);
　　　　}
　　　　return f2;
　　}

　　var result=f1();

　　result(); // 999

　　nAdd();

　　result(); // 1000
 ```
 在这里，result实际上是f2,因为result被赋予了全局变量，始终存在内存当中，而f2中的变量又是依附于f1，f1中的变量不会被销毁，一直存在


 3.for循环使用定时器打印问题

 - 影响：变量会常驻内存，得不到释放，闭包不要乱用 

## &05.this ##
题目：this的不同应用场景，如何取值

纯粹的函数调用、作为函数方法调用、构造函数调用、bind/call/apply调用、ES6 class调用、箭头函数

this取值是在调用时决定的
```bash
    const person = {
		name: 'as',
		say() {
			console.log(this)
		},
		wait() {
			console.log(this);    //this指向当前调用对象
			setTimeout(function() {
				console.log(this);   //this == window--此时是setTimeout本身触发的执行，相当于window在直接调用
			})
		},
		waitAgain() {
			console.log(this);     //this指向当前调用对象
			setTimeout(() => {
				console.log(this);    //this == person--箭头函数中的this，指向上一级作用域中的this
			})
		}
	}
```

- 手写一个bind函数-改变this的指向
```bash
    function fn1(a, b) {
		console.log(this);
		console.log(a, b);
		return "this is fn1";
	}

	//模拟bind
	Function.prototype.bind1 = function() {
		//将参数拆为数组
		const args = Array.prototype.slice.call(arguments);

		//获取this--数组第一项
		const t = args.shift();
		//fn1.bind中的fn1
		const self = this;

		return function() {
			return self.apply(t, args);
		}
	}
	const fn2 = fn1.bind1({x: 100}, 10, 20);
	const res = fn2();
	console.log(res);
```

## &05.同步和异步 ##
- 同步和异步的区别
    * js是单线程语言，只能同时做一件事情。
    * js和DOM渲染共用同一个线程，因为js可修改DOM结构
    * 遇到等待（网络请求，定时任务）不能卡住--异步，解决单线程等待问题
    * 通过callback回调函数调用
- 手写用promise加载一张图片
- 前端使用异步的场景有哪些
    网络请求、定时任务

## &03.说说前端中的事件流 ##

事件流：页面中接收事件的顺序-事件捕获阶段、处于目标阶段、事件冒泡阶段

## &04.安全类 ##

## &04.js的new 操作符做了哪些事情 ##

## &05.js中的垃圾回收机制 ##
必要性：JavaScript程序每次在创建字符串、对象等都会计算其大小，分配相应的内存空间，最终都会释放这些内存以便再利用。如果没有垃圾回收机制，很容易导致内存分配完，导致系统崩溃。JavaScript解释器可以检测到何时程序不再使用一个对象。

垃圾回收机制方法：标记清除、计数引用

编码可以做的优化

1. 避免重复创建对象

2. 在适当的时候解除引用，是为页面获得更好性能的一个重要方式

3. 尽量避免使用全局变量

## &06.说一下commonjs、AMD和CMD ##

## &07.函数柯里化 ##

