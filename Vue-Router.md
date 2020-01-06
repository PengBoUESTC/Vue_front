## Vue Router 路由管理器

### 前端路由

+ 只完成前端页面的跳转，不会将请求发送到后端，通过哈希值实现，htttp 请求不会包含哈希数据。

+ Vue-Router 使用 ```path-to-regexp``` 作为路径匹配引擎

#### 基本结构

+ 引入 ```vue-router``` 此时包含一个构造函数 ```VueRouter```

+ 通过 ```VueRouter``` 构造函数创建一个实例 ```router```

+ ```VueRouter``` 中的参数为一个对象
~~~javascript
let router = new VueRouter({
	routes:[
		{path : "/login", component : "componentObjectName"},
		{
			path : "register", // 子路由使用相对路径
			component : "componentObjectName", 
			children : [
				{path : "account1", component: "componentObjectName"}
			]
		},
		{path : "/", redirect: "/login"},
		{
			path : "/"， 
			// 相同路由显示多个组件的方式，同时设置 router-view 的name属性
			components : {
				compName : componentObjectName
				...
			}
		}
	]
})
~~~

+ 在 ```html``` 页面中创建一个 容器用于显示不同路由对应的组件 ```<router-view></router-view>```，该元素可以设置```name``` 属性,用于多个组件的同时显示。
~~~html
<router-view></router-view> 
<!-- 默认name 属性为 default  -->
<router-view name = "left"></router-view>
<router-view name = "right"></router-view>
~~~

+ 结束

#### 路由中的关键参数

+ ```$route``` : 该参数

+ ```$router```:

+ ```.router-link-active``` : 

### 动态路由匹配

+ <font color = "yellow">路由的匹配优先级由定义顺序决定</font>

+ 使用动态路径参数实现动态路由匹配， 通过 ```:params``` 进行设置, 如 ```/login/:id``` 可以匹配到 ```/login/user``` 路由，此时 ```id = user```
~~~javascript
let router = new VueRouter({
	routes : [
		{
			// 动态路由参数 通过 ：设置
			path:"/login/:id", component: "componentObjectName"
		},
		{
			// 进行多层路由的动态匹配
			path : '/login/:id/:passwd' , component : "componentObjectName"
		}
	]
})
~~~

+ 动态路由参数能够在 ```$route.params``` 对象中获取

+ <font color = "red"> 当使用路由参数时，由于切换参数不会导致组件的切换（如```/login/user1``` 到 ```/login/user2```），因此组件的生命周期函数不会被调用，可通过 ```watch``` 对组件的 ```$route``` 对象进行监控</font>
~~~javascript
const comp1 = {
	template : "#temp1",
	watch:{
		"$route"(to, from){
			// 监测路由的变化
		}
	}
}
~~~

	- 或者使用 ```beforeRouteUpdate``` 进行监控
	~~~javascript
	const comp1 = {
		template : "#temp1",
		beforeRouteUpdate(to, from, next){
		}
	}
	~~~


#### 捕获所有路由或者 404 路由

+ 使用通配符实现所有路由的匹配 ``` * ```
~~~javascript
const router = new VurRouter({
	routes : [
	// 使用 * 通配符实现任意路由的匹配
		{path : '*' , component : ""},
		{
			path : "/user-*", component: ""
			// 匹配所有以 "user-" 开头的路由
		}
	]
})
~~~

+ 将通配符设置的路由匹配参数放在路由列表的最后，可以实现404路由的匹配

+ 使用通配符 ```*``` 实现的路由匹配 ```$route.params``` 会包含一个 ```pathMatch``` 属性保存 通配符匹配的字符串（```$route.params.pathMatch```）


### 路由的嵌套

+ 为了避免组件的覆盖，需要使用路由的嵌套，在保持原有组件不变的情况下进行组件切换

0. 定义组件

1. 在外层组件中添加占位符 ```<router-view></router-view>```

2. 在定义路由中通过 ```children``` 参数进行子路由的配置
~~~javascript
const router = new VueRouter({
	routes : [
		{
			path : "/login", 
			component : "componentObjectName", 
			children : [
				// 这里子路由使用的是相对路径
				{path : "user1", component : "componentObjectName"},

				// 使用空的路由可以设置父级路由的默认显示组件内容 路由为 /login 时 上级组件中<router-view> 显示的内容
				{path : "" , component : "componentObjectName"}
			]
		},
	]
})
~~~

3. 字路由中的 空路径匹配： <font color = "red"> 空的子路由可以用来设置路由不包含子路由时上级组件中 ```<router-view>``` 显示的组件内容</font>


### 导航

+ 导航的基本设置方式有两种

	- ```<a href = "#/login"> 登录 </a>```

	- ```<router-link to = "/login" tag = "a"> 登录 </router-link>```

#### 编程式的导航 

+ 借助 ```router``` 实例的方法通过编程来实现

	- ```router.push(location, [onComplete,] [onAbort])```

	- ```router.replace(location, [onComplete,] [onAbort])```

1. ```router.push()```

	- 在Vue 实例内部可通过 ```this.$router.push ``` 进行调用

	- ```router.push()``` 会向 ```history``` 栈添加一个新的记录，

	- 当点击 ```<router-link to = "/"></router-link>``` 时会在内部调用 ```router.push()``` 方法

	+ ```router.push()``` location 参数可以是一个 字符串，也可以是一个路径描述对象

		- ```router,push("login")``` : 匹配 /login

		- ```router.push({path : "login"})``` : 匹配 /login

		- <font color = "red">```router.push({name : "login", params : {id : 100}})``` : 匹配 /login/100</font>

		- ```router.push({path : "login", query : {id : 100 }})``` : 匹配 /login?id=100

	+ 注意 <font color = "red"> 当在参数对象中设置了 path 属性时， params 属性将不会生效，因此应使用 name 属性代替 path 属性</font>

		- ```router.push({path : "login", params : {id : 100}})``` ： <font color="red"> 这里设置了path 参数，因此 params 不会生效，因此匹配的路径为 /login</font>

		- ```router.push({name : "login", params : {id : 100}})``` : 匹配路径为 /login/100

		- ```router.push({path : `login/$ID`})``` ： 匹配路径为 /login/100

2. ```router.replace()```

	+ 该方法调用与 push 类似，但是该方法不会在 history 中添加一条记录，而是替换一条记录

3. <font color = "red"> 以上两个方法中的 ```onComplete``` 参数与 ```onAbort``` 参数为可选参数</font>
	
	+ ```onComplete``` : 导航成功完成时的回调函数

	+ ```onAbort``` ： 导航终止时调用的回调函数

4. <font coloe = "red">若只是路由参数发生了变化 仍需要通过 ```beforeRouteUpdate``` 进行监测</font>

5. ```router.go(num)``` : 该方法接收一个整数，表示在history 中前进或者后退几步， 类似 ```window.history.go(num)```

6. 以上三种 API 都是仿效浏览器操作 history 的 API 实现的

### 命名路由与命名视图

+ 即通过 ```name``` 属性进行命名

#### 命名路由

+ 为 路由添加```name``` 属性
~~~javascript
const router = new VueRouter({
	routes:[
		{
			path: "/login",
			name : "login",
			component: ""
		},
	]
})
~~~

+ 为 ```to``` 属性传递 ```name``` 属性, 以下两种方式都会匹配 /login/100， 作用是完全一致的
~~~html
<router-link to = "{name : 'login', params : {id : 100 }}"></router-link>
~~~
~~~javascript
router.push({name : "login", params : {id : 100}})
~~~

#### 命名视图

+ 通过 为 ```<router-view>``` 添加 ```name``` 属性
~~~html
<router-view><router-view>
<!-- 默认 name 属性为 default -->
<router-view name = "a"><router-view>
<router-view name = "b"><router-view>
~~~

+ 此时对应的路由中的 组件属性应为  ```components``` **对象**
~~~javascript
const router = new VueRouter({
	routes : [
		{
			path : "/",
			components : {
				default : header,
				a : sidebar,
				b : main
			}
		}
	]
})
~~~

+ **与子路与结合实现嵌套的命名视图布局**

### 重定向与别名

+ 通过 ```redirect``` 参数实现重定向

+ 通过 ```alias``` 参数命名别名

### 路由组件传参