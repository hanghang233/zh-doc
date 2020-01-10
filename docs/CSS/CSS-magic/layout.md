---

hideFooter: true

---
# CSS灵活的背景定位 #

::: tip 契子
- 出于《CSS揭秘》
- css
:::

## &01.自适应内部元素 ##

- 通常，父元素都是根据子元素的内容长度决定。如果我们希望父元素的宽度能根据子元素的宽度进行自适应。常常有以下几种手段：

1.使用浮动
```bash
.div {
    float: left;    # 会改变布局模式
}
```

2.设置display:inline-block（具有收缩特性，当文字超过一行的时候，会换行显示）

3.设置width:min-content

- width:

    1.max-content:不会换行，父元素根据子元素的宽度被撑开

    2.min-content:采用内部元素最小宽度值最大的那个元素的宽度作为最终容器的宽度。

    参考地址：https://www.zhangxinxu.com/wordpress/2016/05/css3-width-max-contnet-min-content-fit-content/

## &02.满幅的背景，定宽的内容 ##

- 背景宽度满幅，内容宽度固定

1.常见写法：定义两层div元素
```bash
<div class="wrapper">
    <div class="footer">this is fofooterfooterfooterfooteroter</div>
</div>  
.footer {
  background-color: aquamarine
}
.wrapper {
  max-width: 300px;
  margin: auto;     #auto：左右自动调整大小
}
```

2.使用calc函数计算：允许以简单的算术来指定属性的值

```bash
<div class="footer">
    <div>this is foooter</div>
    <div>this is foooter</div>
    <div>this is foooter</div>
    <div>this is foooter</div>
    <div>this is foooter</div>
</div>
.footer {
  padding: 0 calc(50% - 350px);
  background: #333;
  color: aliceblue
}
```

<!-- ![blog-home](./img/layout/footer.png) -->

## &03.垂直居中 ##

- margin/padding/top/left百分比的值相当于最近父元素的宽度

1.display: flex；---常用

2.基于绝对定位，结合之前的calc函数计算(但是这里有一个限制，必须定宽)；

```bash
.hello {
  position: relative;
  width: 400px;
  height: 400px;
  border: 1px solid #ccc;
}
.demo2 {
  position: absolute;
  top: calc(50% - 100);
  width: 200px;
  height: 200px;
  background-color: #42b983;
}
```

3.使用transform属性

```bash
.hello {
  position: relative;
  width: 400px;
  height: 400px;
  border: 1px solid #ccc;
}
.demo2 {
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);   #可以不用定宽
  left: 50%;
  width: 200px;
  height: 200px;
  background-color: #42b983;
}
```

- vw:与视口宽度相关, 1vw相当于1%的视口宽度；vh:与视口长度相关

## &04.紧贴底部的页脚 ##

常见布局，要求footer紧贴底部，如果页面过长可以正常显示，如果页面内容篇幅较短，footer不能紧贴在视口的最底部，而是跟在页面内容的后面。

1.可以使用calc计算content内容的高度

2.使用flex

```bash
.hello {
  display: flex;
  flex-flow:column; 
  min-height: 100vh;   #首先设置包裹层的最小视图高度等于窗体高度
}
.content {
  flex: 1;
}
.footer {
  background-color: #42b983;
  padding: 20px;
}
```

## &05.瀑布流布局 ##

1.display：flex

2.网格布局：grid

http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html


