---

hideFooter: true

---
# 其他总结-- #

::: tip 契子
- 
:::

## &01.通信类 ##
- 什么是同源策略及其限制
    不是一个源的文档无法操作另一个不同源的文档：无法操作cookie、无法操作dom、ajax请求不能操作

- 前后端如何通信
    Ajax、WebSocket、CORS

- 如何创建Ajax
    XMLHttpRequest对象的工作流程、兼容处理、事件的触发条件、事件的触发顺序

- 跨域通信的几种方法
    JSONP--利用script异步加载，指定callback函数名，服务器返回之后，执行函数

    Hash--场景是当前页面A，被使用iframe嵌入到页面B，修改iframe.src的值；window.onhashchange

    postMessage--h5通信标准
    
    WebSocket、CORS

## &02.渲染机制 ##
- 什么是DOCTYPE及作用
    直接告诉浏览器什么是DTD，DOCTYPE是用来声明文档类型和DTD规范，一个主要的用途就是文件的合法性验证，如果文件代码不合法，那么浏览器解析时便会出错。

    常见的DOCTYPE有哪些？
    ![npm-script](./imgs/doctype.png)
    HTML4.01有严格模式和宽松模式；

- 浏览器渲染过程
    1. 浏览器解析元素，通过html parser将元素解析为DOM Tree；
    2. 浏览器解析css，通过css parser将css解析为style Rules；
    3. Dom Tree和style Rules结合，生成Render Tree(这里只是包含结构和样式，并不包括内容和元素位置等具体信息)；
    4. Render Tree与Layout结合，教给浏览器paint

- 重排reflow
    reflow概念；什么情况触发reflow；什么情况避免触发reflow--尽量避免修改到dom元素位置结构等；
- 重绘Repaint
    定义；触发条件；如何最小程度的repaint--尽量创建fragment片段，一次性添加到页面中
- 布局Layout

## &03.js渲染机制 ##
- 如何理解js单线程
    一定时间只能执行一项任务，不能执行多项任务

    原因：作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM，这决定了它只能是单线程，
- js任务队列
    为了要执行的代码，就有一个JavaScript的任务队列

    同步任务和异步任务

    异步任务：setTimeout和setInterval 、监听DOM、promise
- Event Loop
    js主线程会不停的从执行栈(同步队列)中读取事件，执行完毕之后，会去查看任务队列是否有任务。

    宏任务队列：setTimeout、setInterval、XHLHttpRequest

    微任务队列：promise

    微任务队列优先级高于宏任务队列

    ```bash
      setTimeout(() => {
        console.log(1);
    }, 0);

    new Promise((resolve) => {
        console.log(2);
        resolve();
    }).then(() => {
        console.log(3);
    });

    console.log(4);   //2、4、3、1
    ```
    ```bash
        let a = () => {
        setTimeout(() => {
            console.log('任务队列函数1')
        }, 0)
        for (let i = 0; i < 5000; i++) {
            console.log('a的for循环')
        }
        console.log('a事件执行完')
        }

        let b = () => {
        setTimeout(() => {
            console.log('任务队列函数2')
        }, 0)
        for (let i = 0; i < 5000; i++) {
            console.log('b的for循环')
        }
        console.log('b事件执行完')
        }

        let c = () => {
        setTimeout(() => {
            console.log('任务队列函数3')
        }, 0)
        for (let i = 0; i < 5000; i++) {
            console.log('c的for循环')
        }
        console.log('c事件执行完')
        }

        a();
        b();
        c();
        // 输出最后的结果
        答案：
        (5000)a的for循环
        a事件执行完
        (5000)b的for循环
        b事件执行完
        (5000)c的for循环
        c事件执行完
        任务队列函数1
        任务队列函数2
        任务队列函数3
        //结果是当a、b、c函数都执行完成之后，三个setTimeout才会依次执行
    ```


    哪些语句放入异步队列

    理解语句进入异步队列的时机

## &04.强缓存与弱缓存 ##

缓存可以简单的分为两种类型：强缓存(200 from cache)和协商缓存(304)

区别简述如下：

    * 强缓存（200 from cache）时，浏览器如果判断本地缓存未过期，就直接使用，无需发起http请求（expired与cache-control）
    * 协商缓存（304）时，浏览器会向服务端发起http请求（带上上一次服务器给你的标记），拿着标记与现在资源的标记进行对比，然后服务端告诉浏览器文件未改变，让浏览器使用本地缓存

对于协商缓存，使用强制刷新就可以让缓存无效；但是对于强缓存，在未过期时，必须更新资源路径才能发起新的请求。

完整的协商缓存过程：发请求-->看资源是否过期-->过期-->请求服务器-->服务器对比资源是否真的过期-->没过期-->返回304状态码-->客户端用缓存的老资源。

## &05.页面性能 ##

https://www.cnblogs.com/smjack/archive/2009/02/24/1396895.html

- 题目：提升页面性能的方法有哪些

    1、压缩资源合并，减少HTTP请求--

    2、非核心代码的异步加载--异步加载的方式--异步加载的区别

    异步加载的方式：1）动态脚本加载；2）script标签添加defer；3）script标签添加async

    异步加载的区别

        1）defer是在HTML加载完成之后才会执行，如果是多个，按照加载的顺序依次执行；

        2）async是在加载完之后立即执行，如果是多个，执行顺序和加载顺序无关；

    3、使用浏览器缓存--缓存的分类--缓存的原理

    缓存的分类--强缓存和协商缓存

    强缓存：expires、cache-control，如果两个都设置，以cache-control为准

    协商缓存：Last-Modified、If-Modified-If-Modified-Since、Etag、If-None-Match

    - 比如：静态资源加hash后缀，根据文件内容计算hash；url和文件内容不变，则会自动触发http缓存机制，返回304

    4、预解析DNS--dns-pre-fetch，大部分浏览器的a标签，默认打开预解析；但是如果连接是https，很多浏览器关闭了预解析

    5、使用CDN：如果一个网站用了n个js、n个css，n个图片，把资源放在cdn上，可以更快的把资源解析出来（根据区域来做服务器的处理）

    6、减少资源体积：压缩代码

    7、减少访问次数：合并代码、SSR服务器端渲染、缓存

    8、节流、防抖
    - 防抖
    场景一：监听一个输入框，文字变化后触发change事件--在一定时间之后，用户没有操作，js才会执行
    ```bash
    //防抖
	function debounce(fn, delay =500) {
		//timer是闭包中--不能被别人修改
		let timer = null;
		return function() {
			if(timer) {
				clearTimeout(timer);
			}
			timer = setTimeout(() => {
				fn.apply(this, arguments);
				timer = null;
			}, delay)
		}
	}
    ```
    - 节流：使得一定时间内只触发一次函数
    拖拽一个元素时，要随时拿到该元素被拖拽的位置

## &6.从输入url到渲染出页面的整个过程 ##
- 加载资源的形式
    html代码、媒体文件、JavaScript css
- 加载资源的过程
- 渲染的过程

1、DNS解析：域名-》IP地址

2、浏览器根据IP地址向服务器发起http请求

3、服务器处理http请求，并返回给浏览器

## &7.window.onload和DOMContentLoaded的区别 ##
- load：页面的全部资源加载完才能执行，包括图片等资源
- DOMContentLoaded：页面资源加载完，图片资源等可能没有加载完成

## &8.安全 ##
- 常见的web前端攻击方式有哪些
xss跨域请求攻击：插入script标签，通过js获取cookie信息

XSRF：用户点击三方链接，携带自身的身份信息--预防：使用