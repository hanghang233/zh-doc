---

hideFooter: true

---
# 动画 #

::: tip 契子
- 出于慕课网教学--七夕css动画
- css
:::

## &01.技术点分析 ##

1. 如何做rem布局

2. 帧动画的原理

3. 如何做3D视角

4. canvas绘图

5. svg绘图

6. 设计模式与异步代码梳理 

## &02.如何做rem布局 ##

rem是一个相当单位，相对于根元素的html的font-size层进行计算。默认情况下浏览器的字体大小是16px，所以1rem = 16px；

自适应处理：使用rem布局的时候,为了兼容不同的分辨率,我们应该要动态的修正根字体的大小,让所有的用rem单位的子元素跟着一起缩放,从而达到自适应的效果

```bash
var docEl = document.documentElement,
    //当设备的方向变化（设备横向持或纵向持）此事件被触发。绑定此事件时，
    //注意现在当浏览器不支持orientationChange事件的时候我们绑定了resize 事件。
    //总来的来就是监听当然窗口的变化，一旦有变化就需要重新设置根字体的值
    resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
    recalc = function() {
        //设置根字体大小
        docEl.style.fontSize = 20 * (docEl.clientWidth / 320) + 'px';
    };

//绑定浏览器缩放与加载时间
window.addEventListener(resizeEvt, recalc, false);
document.addEventListener('DOMContentLoaded', recalc, false);
```

## &03.帧动画的原理 ##

1.animation：step函数

https://www.imooc.com/code/10548

## &04.自适应雪碧图 ##

