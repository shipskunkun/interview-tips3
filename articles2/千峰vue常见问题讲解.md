## review 
> 实现的原理是什么

在created的时候，将需要缓存的 vnode 节点放到 cache 中

在render的时候，根据 name 进行取出

一般情况下和路由匹配使用


```
routes: [{
	path:
	name:
	component:
	meta: {
		keepAlive: true
	}
	
}]

```

## 04、06、07、08、09
略

## 01  axios

[axios中文文档](http://www.axios-js.com/zh-cn/docs/)


axios是什么，怎么使用，实现登陆功能流程？

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。	
主要用于向后台发送请求的，还有就是在请求中做更多的控制  

	从浏览器中创建 XMLHttpRequests
	从 node.js 创建 http 请求
	支持 Promise API
	拦截请求和响应
	转换请求数据和响应数据
	取消请求
	自动转换 JSON 数据
	客户端支持防御 XSRF


提供了并发方法

```javascript
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
	.then(axios.spread(function (acct, perms) {
	// 两个请求现在都执行完成
}));
```

拦截请求和响应

```
//http request 拦截器
axios.interceptors.requst.use(function(config) {

	if(config.method == 'post') {
		config.data = qs.stringify(config.data)
	}
})

//http response 拦截器
axios.interceptors.response.use({
})
```

	

axios fetch ajax 区别  
前两者都是支持promise的语法，后者默认是使用callback   
fetch本质上脱离的xhr是新的语法，有自己的特性 默认不穿 cookie 不能像 xhr 一样去监听请求的进度  
 
 [fetch](https://blog.csdn.net/weixin_38858002/java/article/details/81561972)

	fetch是基于promise实现的，也可以结合async/await。
	fetch请求默认是不带cookie的，需要设置fetch（URL，{credentials:’include’})。
	Credentials有三种参数：same-origin，include，*
	服务器返回400 500 状态码时并不会reject，只有网络出错导致请求不能完成时，fetch才会被reject。
	所有版本的 IE 均不支持原生 Fetch。
	fetch是widow的一个方法；
ajax

	是XMLHTTPRequest的一个实例；
	只有当状态为200或者304时才会请求成功；
 
 
 




Q: 跨域问题如何解决
	
	


## 02、03 vuex

Q: vuex 是什么，怎么使用，哪些功能场景使用它？  
	
	是vue的状态集管理，主要为了解决组件间状态共享的问题，方便状态的维护
	
	原先是组件与组件间传递，变成组件与仓库之间的传递，现在是集中式管理。
	
	并不适用于所有的项目，主要是便于维护，便于解耦
	
	如果不是构建大型项目，使用 vuex 反而是项目代码繁琐多余



Q: vuex 的核心

state mutations getters actions modules

getters: 类似于 computed, 	
modules: 拆分仓库，state拆分成几个module


Q: 使用
 
```
 var store = new Vuex.store({
 	state: {
 		count:0
 	}
 })
 
 
 var vm = new Vue({
 	el:
 	store,
 	data: 
 	methods: {
 	add: function(){
 		store.commit('add')
 	}
 })
 }
```


## 05 导航钩子有哪些？它们有哪些参数？

导航钩子 = 路由的生命周期函数

分为两种， 全局和局部

	全局钩子函数：
		beforeEach： 在路由切换开始时调用
		afterEach： 在路由切换离开时调用
	局部钩子函数：
		beforeEnter：局部到单个路由
	组件的钩子函数
		beforeRouterEnter
		beforeRouterUpdate
		beforeRouterLeave

Q: 如何使用？

```
const router = new VueRouter({
	router: routes
})

const routes = [{
	path: '/',
	name: '/',
	component: Home,
	beforeEnter: function(){
	
	}
}, {
	path: '/buy',
	name: 'buy',
	component: Buy,
	meta: {
		auth: true
	}
}
...
]
	
// 全局路由
// to 即将进入目标对象
// from: 当前导航要离开的导航对象
// next: 是一个函数，调用 resolve 执行下一步

router.beforeEach(from, to, next) {
	if(to.meta.auth) {
		if(token) {
			next()
		}
		else {
			next({
				path: '/login'
			})
		}
	}
}


```

和axios的拦截器有什么区别？

 一个作用于路由，一个作用于请求




## 10 swiper插件从后台获取数据没问题，CSS代码也问题，但是图片不动，应该怎么解决

使用watch？
option: auto

原因: swiper 提前初始化，这个时候数据还没有完全出来

1. vue中专门给提供了一个方法，nextTick(),
 	
	 	把 swiper 的初始化放在 nextTick 中保证后执行
	 	 
	 	this.$nextTick(function(){
	 		var myswiper = new Swiper('.swiper-container', {
	 			autoplay: true,
	 			loop: true,
	 			observer: true,  //修改子元素，会自动初始化 swiper
				observerParents: true // 如果当前swiper父元素发生变动，初始化
	 		})
	 	})
	

2. swiper 的定义， 
		
		observer: true
		observerParents: true 

## 11 路由懒加载


单页面最大的问题，所有页面都是一个js，第一次打开页面，加载会非常慢

为了避免这种情况，我们使用懒加载


路由懒加载，也叫延迟加载，即在你需要的时候加载，减少首页加载时间
结合vue的异步组件和webpack代码分割，实现路由懒加载

1. 将异步组件定义为一个promise
2. 使用import动态引入

```
routes: [
	{
		path: '/',
		name:
		component: import('a.vue')
	}
]

webpack:


output: {
	chunkFileName: [name].js
}

```


## 12 关于Vue-loader


什么是vue-loader:

	vue-loader 是一个加载器，能把 .vue 组件转化为 js 模块，方便浏览器读取  
为什么我们要转译这个vue组件
  
	可以动态的渲染一些数据  

对三个标签，都做了优化，
template、script、css
js中可以直接使用es6
style中默认也可以使用sass，并提供作用域
开发阶段，提供热加载，实时更新


## 13 用过插槽吗？用的是具名插槽还是匿名插槽？
	
	
vue 的插槽是一个非常好的东里，slot 说白了就是一个占位的

vue中slot包含了三种，默认插槽（匿名），具名插槽，作用于插槽

匿名插槽就是没有名字的，只要默认的都填到这里

具名插槽，指的是具有名字的


```
// 匿名插槽
<template>
	<div>
		<p>使用插槽</p>
		<slot></slot>
	</div>
</template>

<about>
	<h2>这里是插槽的内容</h2>
</about>

// 具名插槽
<template>
	<div>
		<p>使用插槽</p>
		<slot name="header"></slot>
		<slot></slot>
		<slot name="footer"></slot>
	</div>
</template>

<about>
	<div slot="header">这是头部</div>
	<h2>这里是插槽的内容</h2>  //只要没加slot的，都代表匿名插槽
	<div slot="footer">这是头部</div>
</about>
```



## 14 说说你对Vue虚拟DOM的理解
	
虚拟dom, 使用js 对象模拟dom, 当我们更新虚拟节点时，更新虚拟dom 代码


本质上是优化了diff算法
采用了新旧dom的对比，获取你差异的dom， 一次更新到 你真实的 dom 上
虚拟dom也有自己的缺陷，他更适合批量修改dom
尽量不要跨层级修改dom
设置key, 可以最大程度利用节点

如果没有设置key值，改key值所在的dom会被删除，key存在的情况，会保留dom






## 15 如何立即Vue的MVVM模式？

> mvc

用户通过人机交互，controller ，将用户输入的指令和数据传递给业务模型，model 模型

model模型，进行业务逻辑判断，数据库存取

model通过view 视图，将结果反馈给用户

> mvvm

通过 viewmodel， model 和 view 没有任何关系

在data中定义的数据，model

view: template 中定义的数据

viewmodel: vue 内部实现




## 16 Vue中的keep-alive的作用


把不活动的组件保存，不会销毁

actived, deactived

不会触发 mounted 等

> 什么是 keep-alive

是让不活动的组件活着，提供了 include 和 exclude 两个属性，允许组件有条件的缓存

> 实现的原理是什么

在created的时候，将需要缓存的 vnode 节点放到 cache 中

在render的时候，根据 name 进行取出

一般情况下和路由匹配使用


```
routes: [{
	path:
	name:
	component:
	meta: {
		keepAlive: true
	}
	
}]



<keep-alive> 
	<router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
```














