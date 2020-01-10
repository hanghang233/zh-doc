---

hideFooter: true

---
# 面试题总结 #

::: tip 契子
- 专业技术：CSS、 JS 语法、HTTP 基础、三大框架、浏览器原理和前端安全，与实际业务联系
- 前端性能优化、Node.js 工具链、异常监控和部署
- 对业务系统的理解、研发流程的推进、开发难题的攻关和团队协作的实践
- https://juejin.im/post/5e11ef3b6fb9a0483a135fa7#heading-28
- https://juejin.im/post/5de11deb5188256eaa0ebd5e
- https://juejin.im/post/5df5bcea6fb9a016091def69
- https://juejin.im/post/5e143104e51d45414a4715f7 浏览器性能相关
:::

## &01.Promise ##

- promise微任务队列和执行顺序
- event loop:主线程从“任务队列”中循环读取任务

https://juejin.im/post/5dcb775c518825574d214b89

## &02.浏览器缓存机制 ##

- 缓存是浏览器的一种缓存机制，可以把请求过的web资源（html，js，css）拷贝一份副本存储在浏览器中，并选择请求配置是否使用该副本

缓存作用：减少浏览器带宽/减少服务器压力/减少网络延迟，提高用户体验

- meta可设置页面是否采用缓存

https://juejin.im/post/5dccbfca518825599563c645

## &03.vue--vue中的组件为什么是函数 ## 

vue中的组件是可以被复用的，一个组件被创建好之后，就可能被用在各个地方，而组件不管被复用了多少次，组件中的data都应该是相互隔离，不受影响的。基于JavaScript特性设计。

```bash
var Component = function() {};
Component.prototype.data = {
    message: 'Love'
}
var component1 = new Component(),
    component2 = new Component();
component1.data.message = 'Peace';
console.log(component2.data.message);  // Peace
```

```bash
var Component = function() {
    this.data = this.data()
}
Component.prototype.data = function(){
    return {
        message: 'Love'
    }
}
var component1 = new Component(),
    component2 = new Component();
component1.data.message = 'Peace';
console.log(component2.data.message); 
```

## &03.vue--v-show和v-if的区别 ## 

1.v-show是css级别控制display属性，v-if是控制dom层是否渲染

2.v-show：适用于频繁操作操作dom隐藏/显示，v-if适用于一次性渲染完

3.v-if场景优化：当值为false时，内部组件不会被渲染，所以在特定条件再渲染部分组件的时候，可以将v-if设置为false，有条件时渲染，这样可以让重要的组件先渲染，合理利用，进行性能优化

## &04.vue--web components组件 ##

- 浏览器web components组件：一种定制的，可以在web app和web页面中使用并复用的元素，可适用于任何框架

- 通过Vue CLI3和新的@vue/web-component-wrapper库创建web components十分方便。
@vue/web-component-wrapper库提供了一个通过web componentAPI接入Vue组件的容器。这个容器能够自动代理properties，attributes，events和slot。

https://cli.vuejs.org/zh/guide/build-targets.html

## &05.vue--渲染列表时，“key属性的作用和重要性” ##

- 渲染项目列表时，key属性允许vue跟踪每个vnode节点，key值是必须唯一的

- 因为vue渲染列表是采用“就地复用”的策略，当更改列表时，不是上下移动元素位置，而是使用更新的元素节点来反映更改。重点体现在：列表排序上

## &06.vue--相关源码推荐 ##

1.https://github.com/PanJiaChen/vue-element-admin

2.https://github.com/d2-projects/d2-admin

3.https://juejin.im/post/5df7b9346fb9a0163307451b

https://juejin.im/post/5df9fe6be51d45583c1cc3f7


