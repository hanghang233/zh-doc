---

hideFooter: true

---
# vue面试题总结 #

::: tip 契子
- https://juejin.im/post/5e50f5e0f265da5709701cc8
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
- :is="component-name"

## &08.vue高级特性一  异步组件 ##
- import函数
- 按需加载，异步加载大组件

```bash
components: {
    FormDemo: () => import('XXXX')
  }
```

## &08.vue高级特性一  缓存组件 ##
- keep-alive：vue层级控制，js对象
- 频繁切换，不需要重复渲染
- vue常见性能优化：异步组件、缓存组件、webpack

## &09.vue高级特性一  mixin，抽离公共组件 ##
- 多个组件有相同的逻辑，抽离出来
- Vue3 提出的composition API旨在解决这些问题

问题：
1. 代码来源不明确，不利于阅读

2. 多个mixin可能会出现命名冲突

3. 关系复杂

## &010. vuex的使用##
- 考察state的基本结构设计

## &011. vue-router##
- 路由模式（hash、h5 history）
- 路由配置（动态路由、懒加载）

## &012. vue原理 ##
- 组件化
- 响应式
- vdom和diff
- 模版编译
- 渲染过程
- 前端路由

## &013. vue响应式 ##
组件data的数据一旦变化，立刻触发视图的更新

核心API-Object.defineProperty
- 如何实现响应式
- 缺点（Vue3.0启用Proxy）

Object.defineProperty缺点

    1）深度监听，需要递归到底，一次性计算量大

    2）无法监听新增属性/删除属性（Vue.set Vue.delete）

    3）无法原生监听数组，需要特殊处理

```bash
    const data = {
        'name': 'zzz',
        'age': '21',
        'info': {
            'address': 'city',  //属于深度监听
        },
        'arr': [1,2,3]
    }

	//重新定义数组原型
	const oldArrayProperty = Array.prototype;
	//创建新对象，扩展新方法不会影响原型
	const arrProto = Object.create(oldArrayProperty);
	['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
		arrProto[methodName] = function() {
			updateView();  //触发视图更新
			oldArrayProperty[methodName].call(this, ...arguments);
		}
	})

	//监听对象属性
	function observer(target) {
		if(typeof target !== 'object' || target === null) {
			//不是对象或数组
			return target
		}
		//
		if(Array.isArray(target)) {
			target.__proto__ = arrProto;
		}
		//重新定义各个属性
		for(let key in target) {
			defineReactive(target, key, target[key]);
		}
	}

	//监听对象
	function defineReactive(target, key, value) {
		//深度监听
		observer(value);

		//核心API
		Object.defineProperty(target, key, {
			get() {
				return value
			},
			set(newValue) {
				if(newValue !== value) {
					//设置新值--深度监听
					observer(newValue);

					value = newValue;
					//触发更新视图
					updateView();
				}
			}
		})
	}

	//视图更新
	function updateView() {
		console.log('触发更新视图')
	}

	observer(data);
	console.log(data.name);
	// data.age = '10';
	data.arr.push('333');   //数组触发
```

## &013. 虚拟DOM和diff ##
- vdom是实现vue和react的重要基石
    vdom-用JS模拟DOM结构，计算出最小的变更，操作DOM
- diff算法是vdom中最核心、最关键的部分--时间复杂度O(n)
    1）只比较同一层级，不跨级比较

    2）tag不相同，则直接删除重建，不再深度比较

    3）tag和key，两者都相同，则认为是相同节点，不再深度比较

## &014. 模板编译 ##
- with函数，
    使用with，能改变{}内自由变量的查找方式，当作obj的属性来查找

## &015. vue面试 ##
- 为何在v-for中使用key
    必须用key，且不能是indx和random；diff算法中通过tag和key来判断，是否是sameNode；
    高效的更新虚拟DOM，提高性能

    就地复用策略：列表数据修改的时候，他会根据key值去判断某个值是否修改，如果修改，则会重新渲染这一项，否则复用之前的元素；

    不建议使用index；场景：v-for一个数组，如果中途插入一个元素，会改变这个元素之后的key值，造成无意义的渲染。建议用id这种不会变的值作为key

- 双向数据绑定v-model的实现

- 对mvvm的理解

- computed有何特点
缓存，data不会不会重新计算，提高性能

- 为何组件data必须是一个函数
定义的.vue最后是一个类，每次使用.vue是使用一个实例化，data必须是一个函数，形成闭包，对外不可见，也是为了不被污染

- ajax请求应该放在哪个生命周期
mounted中，dom加载完成之后

- 何时需要使用beforeDestory
解除自定义事件event.$off，清除定时器，解除自定义的DOM事件，如window scroll等

- vue如何监听数组变化
Object.defineProperty 不能监听数组变化；重新定义原型，重写push pop等方法，实现监听；Proxy可以原生支持监听数组变化

- 请描述响应式原理
监听data变化；组件渲染和更新的过程

- Vue为何是异步渲染，$nextTick何用
异步渲染（以及合并data修改），以提高渲染性能；$nextTick在DOM更新完以后，触发回调

- Vue常见性能优化方式
1、合理使用v-show和v-if

2、合理使用computed

3、v-for中加key，以及避免和v-if的使用；因为v-for的优先级要高一些，每次循环之后指向v-if性能会下降

4、自定义事件、DOM事件及时销毁

5、合理使用异步组件

6、合理使用keep-alive

7、data层级不要太深--vue内部data监听相关

8、使用vue-loader在开发环境做模版预编译

9、webpack层面的优化

## &016. vue 初次渲染/更新过程 ##
- 初次渲染过程
1. 解析模版为render函数（或在开发环境已完成，vue-loader）

2. 触发响应式，监听data属性getter setter

3. 执行render函数，生成vnode，patch（elem，vnode）

- 更新过程
1. 修改data，触发setter（此前在getter中已被监听）

2. 重新执行render函数，生成newVnode

3. patch(vnode, newVnode)

## &017. vue的生命周期 ##
- beforeCreate（创建前）：在数据观测和初始化事件还未开始
- created（创建后）：完成数据观测、属性和方法的运算，初始化事件，$el属性还没有显示出来
- beforeMount（载入前）：在挂载之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板、把data里面的数据和模板生成html。注意此时还没有挂载到html页面上
- mounted（载入后）：vm.$el挂载到实例上调用。实例已完成以下：用编译好的html内容替换el属性指向的DOM对象，完成模板中的html渲染到页面上；
- beforeUpdate（更新前）：在数据更新之前调用
- updated（更新后）：组件DOM已经更新
- beforeDestory（销毁前）
- bestoryed（销毁后）

生命周期的作用：它的生命周期有多个事件钩子，让我们在控制整个VUE实例的过程中更容易形成好的逻辑

## &018. vue实现数据双向绑定的原理 ##
主要：采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty来劫持各个属性的setter，getter

过程：
1. 当初始化vue data时，vue将遍历它的每一个属性，用Object.defineProperty将他们转为getter、setter

2. 当只想data 的get时，会将此属性加入到依赖收集当中，如果下次此属性setter触发变化，监听会通知watcher，从而更新视图

注意：object.defineProperty监听不了数组 ，vue里面重写了数组方法达到监听效果

检测变化的注意事项：Vue无法检测到对象属性的添加或删除，由于Vue会在初始化实例时对属性执行getter/setter转化，所以属性必须在data对象上才能让vue将它转为响应式；如果需要，使用Vue.set/delete


```bash
var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的

```
## &019. 什么是虚拟DOM？为什么虚拟DOM能提升性能 ##
虚拟DOM就是一个JavaScript对象，通过这个JavaScript对象来描述真实DOM；

真实DOM的操作，一般都会对某块元素的整体重新渲染

采用虚拟DOM的话，当数据变化的时候，只需要局部刷新变化的位置就好了；虚拟DOM进行频繁修改，然后一次性比较并修改真实DOM中需要改的部分，最后并在真实DOM中进行排版与重绘，减少过多DOM节点排版与重绘损耗

## &020. vue无法监听数组变化的情况 ##
1. 利用索引值直接设置数组时

解决：Vue.set 或者 vm.arr.splice

2. 修改数组的长度时

解决：vm.items.splice(newLength)

## &021. computed和watch的区别和运用的场景 ##
computed：计算属性，依赖于其他的属性值，并且computed的值有缓存，只有它依赖的属性值发生改变，下一次获取computed的值时才会重新计算computed的值

watch：更多的是观察作用，类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作

场景：

1. 需要进行数据计算，并且依赖于其他数据时；可以利用computed的缓存特性，避免每次获取值时，都要重新计算

2. 当数据变化时执行异步或开销较大的操作时，使用watch

## &022. vue-router路由模式有几种 ##
vue-router有3种路由模式：hash、history、abstract；

hash：使用URL hash值来做路由，支持所有浏览器，包括不支持HTML5 History Api的浏览器
- hash值的改变，都会在浏览器的访问历史增加一个记录，因此我们能通过浏览器的回退、前进按钮控制hash的切换
- 可以使用hashchange事件来监听hash值的变化，从而对页面进行跳转

history：依赖HTML5 History API和服务器配置
- history.pushState()和history.replaceState()，这两个API可以在不进行刷新的情况下，操作浏览器的历史记录

## &023. vue-router中的导航钩子 ##
三种方式可以植入导航
1. 全局导航

router.beforeEach(to, from, next)/router.afterEach(to, from)

2. 单个路由独享

在路由配置时直接定义

3. 组件级

beforeRouterEnter、beforeRouteUpdate、beforeRouteLeave
