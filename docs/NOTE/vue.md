---

hideFooter: true

---
# vue面试题总结 #

::: tip 契子

:::

## &01.v-show和v-if的区别 ##

## &02.为何v-for需要key ##
- v-for的key最好不要用random或者index
- v-for和v-if不能一起使用
- 遍历对象可以使用v-for

## &03.事件 ##
- 【观察】事件被绑定到哪里
- 事件修饰符，按键修饰符
- event参数，自定义参数
vue对象本身就可以实现eventBus的功能，用于兄弟组件之间传值。$emit传递事件，$on接受事件，接受之后必须在beforeDestroy中使用$off及时销魂事件，否则可能会造成内存泄漏

## &04.生命周期 ##
- 挂载阶段
1、created和mounted有什么区别？

created指实例初始化完成，页面还没有开始渲染；mounted指页面渲染完成，数据已挂载到页面上；

- 更新阶段
- 销毁阶段
解除绑定自定义事件、销毁子组件以及监听器

- 生命周期（父子组件）
父created=》子组件created=》子组件mounted=》父组件mounted：先保证子组件渲染完，再渲染子组件

## &05.vue高级特性 ##
- 自定义v-model
- $nextTick/refs
- slot
- 动态、异步组件
- keep-alive
- mixin

## &06.vue高级特性一  v-model ##
- input标签中，@input(v-on:input)input框中输入每个字符都会调用这个事件，多用来做实时查询


## &07.vue高级特性一  $nextTick ##
- vue是异步渲染：data改变之后，dom不会立刻渲染
- $nextTick会在DOM渲染之后被触发，以获取最新DOM节点
- 页面渲染时会将data的数据做整合，多次data修改只会渲染一次


## &07.vue高级特性一  slot ##
- 具名插槽、作用域插槽
作用域插槽：父组件可以获取子组件slot定义的数据，v-slot

## &08.vue高级特性一  动态组件 ##