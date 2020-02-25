---

hideFooter: true

---
# css--面试总结 #

::: tip 契子
- 通用前端组件
:::

## &01.请简单描述一下css盒模型 ##

描述：css盒模型指用来装页面上元素的矩形区域。css的盒模型包括IE和模型和标准盒模型。IE盒模型的width=width+padding+border；标准盒模型的width=元素width

在css3中引入了box-sizing的属性，box-sizing:content-box表示标准的盒模型，box-sizing:border-box表示IE盒模型；box-sizing:padding-box，这个属性的宽度包含了左右padding+width

1. 标准盒模型+IE盒模型
2. 标准盒模型和IE盒模型区别
3. CSS如何设置这两种盒模型--dom.style.width/height、dom.currentStyle.width/height(只有ie支持)、window.getComputedStyle(dom).width/height(通用性更好)
4. JS如何获取盒模型对应的宽和高
5. 实例题（根据盒模型解释边距重叠）
6. BFC（边距重叠解决方案）

## &02.画一条0.5px的线 ##

描述：对于值为小数点的处理，不同的浏览器有不同的处理差异。chorme会把小于0.5的当作1来处理，而FireFox会把小于0.55当作1来处理，Safri是把小于0.75当成1。进一步观察，在手机ios上是可以画出0.5px的边，安卓不行。

解决一：scale
```bash
.hr.scale-half {
    height: 1px;
    transform: scaleY(0.5);
    transform-origin: 50% 100%;
}
```

解决二：线性渐变linear-gradient
```bash
.hr.gradient {
    height: 1px;
    background: linear-gradient(0deg, #fff, #000);
}
```

解决三：boxshadow
```bash
.hr.boxshadow {
    height: 1px;
    background: none;
    box-shadow: 0 0.5px 0 #000;
}
```

解决四：viewport--注意只有移动端才有作用
```bash
<meta name="viewport" content="width=device-width,initial-sacle=0.5">
```

## &03.link标签和@import标签的区别 ##

1. link属于html标签，没有兼容性，而@import是css提供，IE5以上才能识别

2. 页面被加载时，link会被同时加载，@import是等到页面数据加载完之后加载

3. link的权重要高于@import

## &04.BFC块级格式化上下文 ##

BFC：独立的渲染区域，并且有一定的布局规则。BFC是一个独立容器，子元素不会影响到外面。

创建BFC的方式：
- html的根元素
- float浮动
- 绝对定位absolute/fixed
- overflow为hidden
- display为表格布局或弹性布局
- contain值为layout
- content或 strict的元素等。
BFC的作用：
- 清除浮动，解决元素高度塌陷问题：父元素overflow:hidden---BFC子元素即使是float，也会参与高度计算
- 解决元素margin重叠问题：使其中一个元素和另一个元素不在同一个BFC中
- 解决一侧自适应问题
```bash
.left1 {
    width: 300px;
    height: 100px;
    background-color: aquamarine;
    float: left;
}
.right1 {
    word-break: break-all;
}
```


## &05.页面隐藏元素的三种方法 ##

- display:none， 元素隐藏，并且会改变页面布局，可以理解成在页面中把该元素删掉
- visibility:hidden，该元素隐藏起来，不会改变页面布局，不会触发该元素的点击事件
- opacity:0，元素隐藏，不会改变页面布局，但是元素绑定事件的话，会被触发

## &06.设置三栏布局--左右定宽，中间层自适应 ##

- float解决：一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。浮动元素之后的元素将围绕它。

参考：https://www.cnblogs.com/duw76/p/10042999.html
- 使用position
- 使用table
- 使用flex

## &07.圣杯布局 ##
- 参考：https://www.cnblogs.com/Wu-W-Sen/p/4520972.html
- margin-left：百分比，相对于父元素
- left/top：百分比，相对于自身
- margin负边距：css盒子中真正的边界是由margin决定的，margin负边距不仅能影响元素在文档流的位置，还能增加元素的宽度

    1. 负边距即margin的四个边界值为负值。
    2. 在html中使用负边距margin-left和margin-top相当于是元素左移、上移，同时文档流的位置会发生变化。

- 要点
    1. 两边固定，中间自适应
    2. 中间部分在dom结构上优先，以便先渲染
    3. 允许三列中的任何一列成为最高列
    4. 只需要使用一个额外的div标签

```bash
.container {
			border: 1px solid #ccc;
			padding-left: 200px;
			padding-right: 100px;
			float: left;
		}
		.left {
			width: 200px;
			height: 100px;
			background-color: aqua;
			float: left;
			margin-left: -100%;
			position: relative;
			right: 200px;
		}
		.center {
			height: 100px;
			background-color: aquamarine;
			width: 100%;
			float: left;
		}
		.right {
			background-color: bisque;
			width: 100px;
			height: 100px;
			float: left;
		}
```

```bash
<div class="container">
    <div class="center">4dwerwerwerwe</div>
    <div class="left">left</div>
    <div class="right">right</div>
</div>
```

## &08.从输入URL到加载的过程梳理 ##

https://segmentfault.com/a/1190000013662126

## &09.重绘与回流 ##

简述：页面呈现流程
- 浏览器把html代码解析成一个Dom树，html中的每一个tag都是Dom树节点
- 浏览器会把所有的样式解析成样式结构体，在解析过程中会去掉浏览器不能识别的样式。比如IE会去掉-moz开头的样式，而firefox会去掉_开头的样式。
- dom tree和样式结构体呈现成树(render tree)，render tree能识别样式，其中每一个node都有自己的style
- 一旦render tree构建完毕，浏览器就可以根据render tree来绘制页面了。

回流与重绘
- 回流：当render tree的一部分或者全部因为元素的规模尺寸、布局，隐藏等改变而需要重新构建
- 重绘：当render tree的一部分需要更新属性，而这些属性只是影响元素的外观，风格，而不影响布局

什么操作会引起重绘与回流
- 添加删除元素（重绘+回流）
- 隐藏元素，display:none（重绘+回流），visibility:hidden（重绘）
- 用户操作，改变浏览器大小等

减少重绘与回流的方法--也就是说减少render tree的操作
- 尽量使用className修改样式
- 使用documentFragment或div元素进行缓存操作
- 对于多次重排的元素，比如说动画。使用绝对定位脱离文档流，使其不影响其他元素

## &10.题目：假设高度已知，请写出三栏布局，其中左栏、右栏宽度各为300米，中间自适应 ##
- 延伸一：布局优缺点
- 延伸二：未知高度、内部元素高度撑开，哪个方案适用
- 延伸三：兼容方案、业务上面哪种布局最适用

1.float
```bash
<style>
	.container {
		height: 600px;
		border: 1px solid #ccc;
		overflow: hidden;
	}
	.left {
		width: 300px;
		height: 600px;
		float: left;
		background-color: aliceblue;
	}
	.right {
		width: 300px;
		height: 600px;
		float: right;
		background-color: aqua;
	}
	.content {
		height: 600px;
		background-color: blueviolet;
	}
	
</style>
<div class="container">
	<div class="left"></div>
	<div class="right"></div>
	<div class="content"></div>
</div>
```
- 注意清除浮动
- 兼容性比较好，注意处理周边元素关系
- 不适合未知高度
- 延伸：浮动原理：相邻元素围绕浮动元素，清除浮动，创建BFC

2.绝对定位
```bash
<style>
	.container {
		height: 600px;
		border: 1px solid #ccc;
		overflow: hidden;
		position: relative;
	}
	.left {
		width: 300px;
		height: 600px;
		background-color: aliceblue;
		position: absolute;
		left: 0px;
	}
	.right {
		width: 300px;
		height: 600px;
		background-color: aqua;
		position: absolute;
		right: 0px;
	}
	.content {
		height: 600px;
		background-color: blueviolet;
		left: 300px;
		right: 300px;
		position: absolute;
	}
	
</style>

<div class="container">
	<div class="left"></div>
	<div class="content"></div>
	<div class="right"></div>
</div>
```
- 快捷
- 布局脱离文档流，子元素也脱离文档流，有效性比较差
- 不适合未知高度

3.flex
- IE8不支持
- 适合未知高度

4.table布局
```bash
<style>
	.container {
		width: 100%;
		height: 600px;
		border: 1px solid #ccc;
		display: table;
	}
	.left {
		width: 300px;
		background-color: aliceblue;
		display: table-cell;
	}
	.right {
		width: 300px;
		background-color: aqua;
		display: table-cell;
	}
	.content {
		background-color: blueviolet;
		display: table-cell;
	}
	
</style>

<div class="container">
	<div class="left"></div>
	<div class="content"></div>
	<div class="right"></div>
</div>
```
- 兼容性好
- 注意：三栏会高度一致，其中一个超出高度，另外两个统一
- 适合未知高度

5.网格布局grid
```bash
<style>
	.container {
		width: 100%;
		height: 600px;
		border: 1px solid #ccc;
		display: grid;
		grid-template-rows: 100px;
		grid-template-columns: 200px auto 200px;
	}
	.left {
		background-color: aliceblue;
	}
	.right {
		background-color: aqua;
	}
	.content {
		background-color: blueviolet;
	}
</style>

<div class="container">
	<div class="left"></div>
	<div class="content"></div>
	<div class="right"></div>
</div>
```
- 兼容性不好
- 不适合未知高度

## &11.题目：上下高度固定，中间自适应 --适用于移动端 ##

1.flex

2.display: table-row
```bash
<style>
	.container {
		width: 100%;
		height: 600px;
		border: 1px solid #ccc;
		display: table;
	}
	.top {
		background-color: aliceblue;
		display: table-row;
		height: 200px;
		width: 100%;
	}
	.bottom {
		background-color: aqua;
		display: table-row;
		height: 100px;
		width: 100%;
	}
	.content {
		background-color: blueviolet;
		display: table-row;
	}
</style>

<div class="container">
	<div class="top">11</div>
	<div class="content">22</div>
	<div class="bottom">33</div>
</div>
```

3.使用绝对定位
```bash
<style>
	.container {
		width: 100%;
		height: 600px;
		border: 1px solid #ccc;
		position: relative;
	}
	.top {
		background-color: aliceblue;
		height: 200px;
		position: absolute;
		top: 0px;
		width: 100%;
	}
	.bottom {
		background-color: aqua;
		position: absolute;
		bottom: 0px;
		height: 100px;
		width: 100%;
	}
	.content {
		background-color: blueviolet;
		position: absolute;
		top: 200px;
		bottom: 100px;
		width: 100%;
	}
</style>

<div class="container">
	<div class="top">11</div>
	<div class="content">22</div>
	<div class="bottom">33</div>
</div>
```

4.网格布局

## &12.DOM事件类 ##

https://www.imooc.com/article/50745

1. 基本概念：DOM事件的级别--
	DOM0--ele.onclick=function(){}
	DOM2--ele.addEventListener = 
	DOM3--增加了很多元素事件，keyup等
2. DOM事件模型--
3. DOM事件流
4. 描述DOM事件捕获的具体流程
5. Event对象的常见应用
6. 自定义事件

## &13.HTTP协议类 ##

1. HTTP协议的主要特点--简单快速(URI固定，处理简单)、灵活(头部分，数据类型)、无连接(连接一次，就会断掉)、无状态(客户端和服务端是两种身份)

2. HTTP报文的组成部分

	包含请求报文+响应报文；请求报文：请求行+请求头+空行+请求体（具体）

3. HTTP方法--GET、POST、PUT、DELETE、HEAD

4. POST和GET的区别
	- GET在浏览器回退是无害的，而POST会再次提交请求
	- GET请求会被浏览器主动缓存，而POST不会，除非手动设置
	- GET请求参数会被完整保留在浏览器的历史记录里面，而POST参数不会保留--涉及CSRF攻击
	https://blog.csdn.net/xiaoxinshuaiga/article/details/80766369
	- GET在URL中请求参数是有限制的，POST没有限制
	- 对参数的类型，GET只接受ASCII字符，而POST没有限制
	- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息

5. HTTP状态码

	- 1XX：指示信息-表示请求已经接收，继续处理
	- 2XX：成功-表示请求已被成功接收
	- 3XX：重定向-要完成请求必须进行更进一步的操作
	- 4XX：客户端错误-请求有语法错误或请求无法实现
	- 5XX：服务器错误-服务器未能实现合法的请求

	301:永久重定向 302:临时重定向 304:浏览器有缓存 400:客户端请求有语法错误 401:请求未经授权 403:页面被禁止访问
	500:服务器发生不可预知的错误原来的缓冲文档还可以继续使用 503:请求未完成，服务器临时过载或当机，一段时间后可能恢复正常

6. 什么是持久连接

	HTTP协议采用“请求-应答”模式，当使用普通模式，即非keep-alive模式，每个请求/应答客户都要新建一个连接。当使用keep-alive(持久连接、连接重用)时，keep-alive功能使得客户端到服务器端连接持续有效，当出现对服务器的后继请求时，keep-alive功能避免了建立或者重新建立连接。HTTP1.1支持持久连接

7. 什么是管线化

	管线化机制通过持久连接完成，仅HTTP1.1支持此技术

## &14.使用display：inline-block会产生什么问题？解决方法？ ##
问题：两个display：inline-block元素放在一起，中间会产生一个空白

原因：元素被当成行内元素排版时，元素之间的空白符会被浏览器处理，在字符不为0的情况，空白符会占据一定的宽度

解决：1、父元素设置font-size为0，子元素再做设置

2、子元素设置float：left

## &15.margin：auto为什么可以实现垂直居中 ##
```bash
div  {
        width: 20px;
        height: 20px;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
}
```
块状水平元素在默认情况下，如果设置了margin-left/margin-right,padding-left/padding-right,border-left-width/border-right-width等，实际内容区域会响应变窄。

但是，当一个绝对定位元素，其对立定位方向属性同时有具体定位数值的时候，流体特性就发生了。具有流体特性绝对定位元素的margin:auto的填充规则和普通流体元素一模一样，含有以下特性：
- 如果一侧定值，一侧auto，auto为剩余空间大小；
- 如果两侧均是auto, 则平分剩余空间

## &16.input有个自动记忆功能，如何去掉 ##
input有一个autocomplete属性，默认是on，其含义代表是否让浏览器自动记录之前输入的值；off则记录关闭

## &17. 谷歌浏览器默认字体大小##
描述：谷歌浏览器默认字体大小是12，如果要设置小于12的字体怎么办

- 使用-webkit-text-size-adjust:none; 但是注意高版本的chrome不支持
- -webkit-transform:scale();transform:scale()这个属性只为可以缩放可以定义宽高的元素

## &18. 如果用户说页面白屏可能是哪些问题##
1. 打开浏览器查看页面输出，查看源码是否异常，http状态等，本步骤用来检查具体是后段还是前端问题，还是网络问题--弱网下，网络延迟，js加载延迟会阻塞页面

2. 如果是后端问题，后端查看accesslog、程序日志，看看是否有问题

3. 如果是前端问题，那么根据给出的js异常之类的排查

## &19. 前端如何监控错误 ##