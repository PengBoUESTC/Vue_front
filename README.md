## Vue 学习 webpack 学习

### Vue 的基本使用结构

1. 引入 ```Vue.js```
~~~html
<script src = ""></script>
~~~

2. 创建一个容器，该容器被 ```Vue``` 的实例控制
~~~javascript
let vue = new Vue({
    el: "#app",
    data: {},
    methods: {},
    ...
})
~~~

3. 创建一个 ```Vue``` 实例，并通过 其中的 ```el``` 属性进行容器的绑定

4. 在该实例中定义 ```data```、```methods```、```computed```、```filters``` 等属性

### Vue 基本指令

#### 数据绑定

1. ```{{ data }}```

+ Mustache 语法，将 data 替换为 Vue 实例属性 ```data``` 的数据, 将数据解析为普通文本。

+ 可以在 ```{{ }}``` 中使用**单个表达式**
~~~html
<span> {{ data + 1 }} </span>
<span> {{ data? data1: data2 }} </span>
~~~

+ 不能用在标签**属性**上

2. ```v-text```

+ 用于显示

3. ```v-html```

+ 将数据按照 ```html``` 代码格式转换显示
~~~html
<span v-html="data"></span>
~~~

+ 动态渲染 ```html``` 可能容易遭受 ```xss``` 攻击

4. ```v-bind```

+ 标签属性数据绑定
~~~html
<div v-bind:id = "data">...</div>
~~~

+ 简写为 ```:```
~~~html
<div :id = "data"></div>
~~~

#### 指令

> 指令预期的值是单个表达式（```v-for``` 例外），职责是 <font color = "yellow">当表达式发生变化时，响应式的将数据作用于DOM中</font>

1. ```v-if ``` 

+ 根据表达式的 ```true``` or ```false``` <font color = "green">插入或移除(DOM 操作)</font>元素
~~~html
<p v-if = "seen"> 根据 seen 的值进行元素是否插入的控制</p>
~~~

2. ```v-show``` 

+ 与 ```v-if``` 相似，但是，```v-show``` 是通过控制元素 ``` display : none``` 属性进行元素显示与否的控制的, 不会进行DOM 操作
~~~html
<p v-show = "show"> 当 show 的值为 false 时，该标签的 display 属性将设置为 none </p>
~~~

3. ```v-for```

+ 基于一个数组来渲染一个列表
~~~html
<li v-for = "item in items"> {{ item }} </li>
<li v-for = "(item, index) in items"> {{ item }} </li>
~~~

4. ```v-bind```

+ 元素属性绑定，后要有参数
~~~html
<div v-bind:id = "id"> </div>
~~~

+ 同时支持 表达式作为 指令的参数 <font color = "red"> 表达式形式的参数需要使用 '[  ]' 包裹 </font>
~~~html
<div v-bind: [attributeName] = "parData"> </div>
~~~

5. ```v-on```

+ 进行事件绑定，需要接收一个事件句柄参数
~~~html
<button @click = "add"> + </button>
~~~

+ 同时支持表达式形式的参数
~~~html
<button @[eventName] = "handler"> + </button>
~~~

> 表达式形式的指令参数，返回值为一个字符串或者 ```null```, ， ```null``` 值可以用于显式的移除属性绑定，其他返回值都会触发一个警告。

#### 修饰符

+ 事件修饰符

+ 按键修饰符

### Vue 生命周期

+ Vue 生命周期可以分为四部分： 创建（create）、挂载（mount）、更新（upddate）、销毁（destory）。

+ [生命周期](../lifecycle.png)

<!-- + <img src = "../lifecycle.png" width = "40%" height = "40%" style="transform:rotate(270deg)"> -->

1. 创建 ```create``` 阶段两个钩子函数

	+ beforeCreate

	+ created: <font color = "yellow"> 使用较多 </font>

	+ <font color = "red">在 ```created``` 阶段之前， ```Vue``` 实例的属性还未被创建，因此不能在此之前进行数据的访问或者方法的调用</font>

2. 挂载 ```mount``` 阶段两个钩子函数

	+ beforeMount

	+ mounted

3. 更新 ```update``` 阶段两个钩子函数

	+ beforeUpdate: <font color = "red">实例中的 ```data``` 更新，但是并未渲染到 ```DOM``` 中 </font>

	+ update: <font color = "red">更新的数据渲染结束</font>

4. 销毁 ```destory``` 阶段两个钩子函数

	+ beforeDestory

	+ destoryed

### 计算属性与侦听器

#### 计算属性 (computed)

+ 模板表达式虽然便利，但是不便于用于复杂的逻辑关系

+ Vue 实例中的 ```computed``` 属性进行方法的声名

	- 计算属性默认情况下只有 ```getter``` 属性

	- 可以显式的定义 ```getter``` 与 ```setter``` 属性

	<font color = "pink"> ```getter ``` 方法，用于获取参数的值, ```setter``` 用于参数赋值</font>

~~~javascript
computed: {
	reverseStr(){
		// 该方法默认为 getter 方法
		return this.meg.split().reverse().join()
	}
}

// 显式的定义 setter 方法 与 getter 方法

computed: {
	fullName: {
		set(){
			// 注意这里不能对 fullName 进行赋值操作，会产生递归调用
			this.firstName = names[0]
			this.lastName = names[1]
		} ,
		get(){
			// get 方法的返回值就是 该 computed 属性的值，因此必须使用 return 返回一个值
			return this.firstName + this.lastName 
		}
	}
}
~~~

+ 计算属性（computed）与方法（methods）

	- 计算属性的值会缓存，当该属性依赖项产生变化时，才会重新进行计算，否则直接调用缓存值

	- 方法每次调用都会执行定义的函数

#### 侦听器 (watch)

+ 更通用的方式用于**观察和响应** 数据的变动
~~~javascript
watch: {
	firstName(){
		this.fullName = this.firstName + this.lastName
	},

	lastName(){
		this.lastName = thsi.firstName + thsi.lastName
	}
}
~~~

### class 与 style 绑定

+ 通常元素属性可通过 ```v-bind``` 进行绑定，但是通常 class 与 style 会比较复杂，因此 Vue对 这两个属性绑定进行了增强

#### HTML class

+ ```v-bind: class = {className: isActive}```, 传递对象，其中 isActive 为一个 data 参数
~~~html
<div v-bind: class = {calss1 : true, calss2: false}> 当对象属性为真时，将该属性对应的类名添加到元素上</div>
<div v-bind : class = "classObject"> 或者直接在 Vue 实例中定义一个对象参数</div>
~~~
~~~javascript
data: {
	isActive: true,
	classObject: {
		class1: true,
		class2: false
	}
}
~~~

+ ```v-bind: class = []``` 传递数组
~~~html
<div v-bind : class = "[classData1, classData2]"> 注意数组中的数据为变量， 不是字符串</div>
~~~
~~~javacript
data:{
	classData1: calss1,
	classData2: class2
}
~~~

#### 内联 style

+ 对象方式
~~~html
<div v-bind: style = "{color: colorData, font: fontData}"> 其中 colorData 与 fontData 为定义在 data 属性上的参数 </div>
<div v-bind: style = "styleObject"></div>
~~~
~~~javascript
data: {
	colorData: red,
	fontData: 12px,
	styleObject: {
		color: red,
		margin: 0px,
	}
}
~~~

+ 数组方式
~~~html
<div v-bind: style = "[]"></div>
~~~

+ 支持多个值
~~~html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
~~~

### 条件渲染

+ ```v-if``` 用于条件性的渲染一块内容，当表达式的值为 truthy 时被渲染， 同时```v-else``` 为其添加一个 else 选择
~~~html
<div v-if = "awesome"> Vue is awesome </div>
<div v-else> Nooo </div>
~~~

+ 当想通过 ```v-if``` 进行多个元素的控制时，可通过 <font color = "red">不可见的 ```template```元素进行包裹</font>
~~~html
<template v-if = "show">
	<ul>
		<li></li>
	</ul>
	<div></div>
</template>
<!-- 渲染后 template 将不会作为一个元素进行显示，仅会显示包裹内容 -->
~~~

+ 新增 ```v-else-if``` 用于更加复杂的逻辑关系

> Vue 会通过复用已有的元素进行渲染，因此可能造成不必要的影响，可通过 <font color = "red">key </font>属性进行控制，指定key 属性的标签不会被复用
~~~html
<template v-if = "isEmail">
	<label>userName </label>
	<input placeholder = "输入邮箱">
</template>
<template v-else>
	<label>phone </label>
	<input placeholder = "输入电话">
</template>
<!-- 以上方式中由于未指定 key 属性，因此 inout 标签与 label 标签会被复用，仅 placeholder 不同，导致数据输入后切换不会刷新输入值 -->

<template v-if = "isEmail">
	<label>userName </label>
	<input placeholder = "输入邮箱" key = "email">
</template>
<template v-else>
	<label>phone </label>
	<input placeholder = "输入电话" key = "phone">
</template>
<!-- 以上方式 input 指定了 key 属性，因此不会被复用，但是 label 标签仍会被复用 -->
~~~

+ ```v-show``` 与 ```v-if```

	- ```v-if``` 通过DOM操作进行元素的显示与否

	- ```v-if``` 是惰性的，即开始表达式为假时，不会进行渲染

	- ```v-show``` 不管表达式的值真假都会渲染，通过控制 ```diaplay``` 属性进行元素显示与否的控制

	- 频繁的切换 ```v-show``` 开销更小

+ 不推荐 ```v-for``` 与 ```v-if``` 结合使用，其中 ```v-for``` 优先级较高

### 列表渲染

+ ```v-for``` 将一个数组数据渲染为一个列表
~~~html
<ul>
	<li v-for = "item in items">{{ item }}</li>
</ul>

<!-- v-for 支持 ES6 中的迭代器的方式 -->
<ul>
	<li v-for = "item of items">{{ item }}</li>
</ul>

<!-- 同时 v-for 支持第二个参数，用于获取循环的索引 -->
<ul>
	<li v-for = "(item,index) in items">{{ item }}</li>
</ul>
~~~
~~~javascript
data: {
	items: [...]
}
~~~

+ ```v-for``` 支持<font color = "red"> 对象遍历 </font>, 会按照 ```Obejct.keys()``` 得到的键值顺序进行遍历
~~~html
<li v-for = "value in object"> pro </li>
<li v-for = "(value, key) in object"></li>
<li v-for = "(value, key, index) in object"> </li>
~~~

+ <font color = "red">v-for 指令使用 “就地更新”策略，即当数据的顺序发生变化时，DOM 元素顺序不会改变，key值可以指明每个节点的身份， 在遍历的元素中指明 key 值 </font>

+ <font color = "red">Vue 可侦听的数组变化 ==> 变异方法</font>

	- push()

	- pop()

	- shift()

	- unshift()

	- splice()

	- sort()

	- reverse()

> 以上方法会改变，调用了该方法的数组本身，因此称为 变异方法

	- 当使用非变异方法时（concat filter slice），需要对数组进行重新赋值，才能使 Vue 检测

+ <font color = "red"> Vue 不能监听的数组操作</font>

	- 通过索引进行数组元素赋值操作 ```arr[index] = newValue```

	- 修改数组长度时 ``` arr.length = newLength```

	- <font color = "green"> 可通过 ```splice()``` 实现以上两种目的，同时保障 Vue 能够监测</font>

+ <font color = "red"> 不能监测对象属性的添加或者删除 </font>

	- ```Vue.set(obj, key, value)```

	- ```vm.$set(obj, key, value)```

	- 添加多个新的属性，通过 ```assign``` 进行赋值操作

		+ vm.obj = Object.assign({}, vm.obj, {pro1, pro2...})

+ ```v-for``` 可接收整数
~~~html
<li v-for = "n in 10 "></li>
~~~

+ 使用 ```template``` 元素进行 块 的循环渲染
~~~html
<template v-for = "n in 10">
	<button>
	<input>
	...
</template>
~~~

### 事件处理

+ ```v-on``` 监听 DOM事件

+ 事件处理方法，定义在 Vue实例 methods 属性中

+ 事件修饰符

	- .prevent : 阻止默认事件

	- .stop ： 禁止冒泡传递

	- .capture ： 使用事件捕获模式

	- .self ： 仅自身为事件对象时触发

	- .once ： 仅触发一次

	- .passive ： 不阻止事件的默认行为，indicates that the function specified by listener will never call preventDefault()

+ 按键修饰符

	- 通过 Vue.config.keyCodes 对象 自定义按键修饰符
	~~~html
	Vue.config.keyCodes = {
		name: keycode
	}

	Vue.config.keyCodes.f1 = 112
	~~~

### 表单输入绑定

+ ```v-model``` 用于实现表单元素的双向绑定，本质上是语法糖，用于监听用户的输入事件，以更新数据

	- <font color = "yellow"> ```v-model``` 绑定的元素，将会忽略 ```value checked selected 等属性的值```</font>

+ ```v-model``` 为不同的元素抛出不同的事件

	- ```text``` 与 ```textarea``` 为 ```value``` 属性 与 ```input``` 事件

	- ```checkbox``` 与 ```radio``` 为 ```checked``` 属性与 ```change``` 事件

	- ```select``` 字段将 ```value``` 作为属性，并将 ```change``` 作为事件

#### 表单

+ 文本
~~~html
<input v-model = "message" placeholder = 'edit'>
~~~

+ 多行文本
~~~html
<textarea v-model = "message" placeholder = "input multiple lines..."> </textarea>
~~~

+ 复选框
~~~html
<!-- 单个复选框，v-model 绑定 布尔值 -->
<input type = "checkbox" id = "checkbox" v-model = "checked">
<label for = "checkbox"> {{ checked }} </label>

<!-- 多个复选框，绑定到同一个数组 -->
<template v-for = "item in items">
	<input type = "checkbox" id = item value = item v-model = "checkNames">
	<label for = "item">{{ item }}</label>
</template>
~~~

+ 单选按钮
~~~html
<input type = "radio" id = "one" value = "one" v-model = "picked">
<label for = "one"> one </label>
~~~

+ 选择框
~~~html
<select v-model = "selected">
	<option disable value = ''></option>
	<option> A </option>
	<option> C </option>
	<option> B </option>
</select>

<!-- 支持多选时（multiple 属性），绑定到一个数组  -->
~~~

#### 修饰符

+ ```.lazy```

	- 默认情况下 ```v-model``` 在每次 ```inout``` 事件触发后， 将输入框中的值与数据进行同步，通过 ```.lazy``` 修饰符转变为 ```change``` 事件触发同步

	- ```<input v-model.lazy = "message">```

+ ```.number```

	- 自动将用户输入值转换为数值类型

	- ```<input v-model.number = "age">```

+ ```.trim```

	- 自动去除输入的首尾空白字符

	- ```<textarea v-model.trim = "message"><textarea>```

### Vue 组件

+ <font color = "red">组件与 Vue 实例接收相同的属性参数，但是 data 属性必须时一个返回对象的方法</font>

+ 每调用一次组件就会产生一个该组件的实例

+ <font color = "yellow">由于组件需要复用，因此组件上的 data 属性必须时一个函数，才能在每次调用时返回一个独立的 data 对象</font>

+ <font color = "red"> 组件只能有一个根节点 </font>

#### 组件注册

##### 全局组件

+ 通过 ```Vue.component``` 定义全局组件
~~~javascript
// 通过 Vue.component 方法将 template 模板字符串定义为一个 componentName 的组件标签
Vue.component(componentName, {
	// 组件的 data 属性必须是一个返回对象的方法 
	data() {
		return {}
	},
	template: ""
})
// 可通过 <componentName></componentName>的方式调用组件
~~~

~~~javascript
const template = ""
Vue.component(componentName, {
	data(){
		return {}
	},
	template
})
~~~

##### 私有组件

~~~javascript
const componentA = {}
const componentB = {}

new Vue({
	el: "#app",
	data: {},
	components:{
		componentA,
		componentB
	}
})
~~~

#### 子组件传值

1. 在子组件上定义 ```props``` 属性
~~~javascript
Vue.component("test", {
	props:['post'],
	template: ""
})
~~~

2. ```props``` 属性中的值作为子组件与父组件之间连接的桥梁，不但是子组件元素的属性，同时其绑定值是向父组件索取的。
~~~html
<test v-bind:post = "superPost"> 这里的superPost 为父组件中 data 的值 <test>
~~~

3. 即若子组件想要使用父组件中的数据，需要使用 ```props``` 属性在自己定义的元素标签上添加属性，并通过属性绑定获取父组件数据

#### 父组件监测子组件的事件

+ 子组件通过内建 的 $emit() 方法触发父组件的监听

+ 子组件可向父组件通过事件传值，父组件通过 ```$event``` 获取传递的值
~~~html
<!-- 在子组件中通过 $emit 进行事件的传递 -->
<button v-on: click = "$emit('superHandler', value)">

<!-- 在父组件中监听子组件传递的事件 -->
<test v-on: superHandler = "... $event">
~~~

#### 动态组件



### 插槽

### Vue 工具