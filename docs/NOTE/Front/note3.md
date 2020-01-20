---

hideFooter: true

---
# h5页面常见ios和android兼容问题 #

::: tip 契子
- 神奇的兼容啊
:::

## &01.ios软键盘调起影响fixed布局问题 ##

问题：ios系统中，如果底部设置button定位为fixed，点击input调起软键盘，之后收起软键盘，会出现按钮点击不了的情况

出现机型：iphone6/7

出现原因：因为在ios下点击input等调起软键盘，整个页面会上移，document.body.scrollOffset会由0变成大于0。整个软键盘收起之后，页面会下移，但是document.body.scrollOffset并不会变成0，所以这时候触控不准；

解决办法：

```bash
$("input").blur(function() {
console.log("失去焦点");
window.scrollTo(0, 0);
});
```

## &02.ios点击input等调起软键盘不灵敏 ##

问题：ios某些机型，触发input/textarea等聚焦事件，调起软键盘不灵敏，需要重按或者长按才能调起软键盘

解决办法：引入fastClick.js文件，修改部分源代码。使用之后，发现依然需要等几百毫秒才能点击，怀疑是因为元素未渲染完直接点击引起，这里做了个loading层，避免发生此情况

https://blog.csdn.net/q95548854/article/details/90267182





