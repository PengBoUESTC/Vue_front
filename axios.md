## axios

+ 易用，简洁且高效的 ```http``` 库

+ 同样的 ```API``` Node 与浏览器都支持

+ 支持 ES6 ```Promise``` 语法，告别```callback```

+ 丰富的配置项，支持拦截器等配置

+ 转换请求与响应数据

+ ...

### axios API

+ ```axios(config)```
~~~javascript
// Post 请求
axios({
	method: "post",
	url: "/user/12345",
	data: {
		firstName: "P",
		lastName: "B"
	}
})

// Get 请求
axios({
	method: "get",
	url: "/user/12345",
	responseType: "stream"
}).then((res)=>{

})
~~~

+ ```axios('/user/12345')```默认 get 请求

+ 请求方式别名

	- 当使用这种方式时，请求方法与请求数据，请求路径不用放在 ```config``` 参数中

	- ```axios.request(config)```

	- ```axios.get(url [, config])```

	- ```axios.post(url [, data [, config]])```

	- ```axios.head(url, [config])```

	- ```axios.options(url, [config])```

	- ```axios.put(url, [data, [config]])```

	- ```axios.delete(url, [config])```

	- ```axios.patch(url, [data, [config]])```

+ 处理并发请求

	- ```axios.all(iterable)```

	- ```axios.spread(callback)```

+ 创建 ```axios``` 实例

	- ```axios.create([config])```
	~~~javascript
	const instance = axios.create({
		baseURL: "http://...",
		timeout: 1000,
		headers: {key: value}
	})
	~~~

	- 实例方法

		+ axios#request()

		+ axios#get()

		+ axios#post()

		+ axios#delete()

		+ axios#head()

		+ axios#put()

		+ axios#options()

		+ axios#patch()

		+ axios#getUri()

### ```config``` 参数配置项

+ 
~~~javascript
{
	url: "" , // 必填项，只用 url 是必填项
	method: "get" , // 默认是 get  方式

	// baseURL 将会前缀到 url 参数上组成最终的 url ,除非 url 是绝对路径
	// 在 axios 实例中可以设置 baseURL 实例，使路由书写更加方便
	baseURL: "" , 

	// transformRequest 可以用来拦截过滤 request 请求，在数据达到服务器之前
	// 仅对 PUT POST PATCH 有效
	// 数组中最后一个方法必须返回一个 字符串 或者 buffer 数组， formdata,Stream 的实例
	transformRequest: [function(data, headers){
		...
		return data	
	}],

	// 在数据传递到 then 或者 catch 之前进行处理
	transformResponse: [function(data){
		...
		return data
	}],

	// 请求头参数
	headers: {},

	// URL 参数
	params: {},

	// 用于序列化 params
	paramsSerializer: function(params){
		return Qs.stringify(params, {arrayFormat: ''})
	},

	// request body
	//  PUT, POST, Patch
	// 当设置了 transformRequest 参数时，data必须是以下数据类型
	//  string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
	//  Browser only : FormData, File, Blob
	//  Node only : Stream, Buffer
	data: {},

	timeout: 1000, //default 为0 五超时时间

	// 是否需要跨域
	withCredentials: false，

	auth: {
		username: '',
		password: ''
	},


	// 服务器响应的数据类型： 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
	responseType: 'json', //default

	responseencoding: "utf8", //default

	// 用作 xsrf token 的cookie 字段
	xsrfCookieName: 'XSRF-TOKEN', //default

	// `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  	xsrfHeaderName: 'X-XSRF-TOKEN', // default

  	// `onUploadProgress` allows handling of progress events for uploads
  	onUploadProgress: function (progressEvent) {
    	// Do whatever you want with the native progress event
  	},

  	// `onDownloadProgress` allows handling of progress events for downloads
  	onDownloadProgress: function (progressEvent) {
    	// Do whatever you want with the native progress event
  	},

  	// `maxContentLength` defines the max size of the http response content in bytes allowed
  	maxContentLength: 2000,

  	// `validateStatus` defines whether to resolve or reject the promise for a given
  	// HTTP response status code. If `validateStatus` returns `true` (or is set to `null`
  	// or `undefined`), the promise will be resolved; otherwise, the promise will be
  	// rejected.
  	validateStatus: function (status) {
    	return status >= 200 && status < 300; // default
  	},

  	// `maxRedirects` defines the maximum number of redirects to follow in node.js.
  	// If set to 0, no redirects will be followed.
  	maxRedirects: 5, // default

  	// `socketPath` defines a UNIX Socket to be used in node.js.
  	// e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  	// Only either `socketPath` or `proxy` can be specified.
  	// If both are specified, `socketPath` is used.
  	socketPath: null, // default

  	// `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
  	// and https requests, respectively, in node.js. This allows options to be added like
  	// `keepAlive` that are not enabled by default.
  	httpAgent: new http.Agent({ keepAlive: true }),
  	httpsAgent: new https.Agent({ keepAlive: true }),

  	// 'proxy' defines the hostname and port of the proxy server.
  	// You can also define your proxy using the conventional `http_proxy` and
  	// `https_proxy` environment variables. If you are using environment variables
  	// for your proxy configuration, you can also define a `no_proxy` environment
  	// variable as a comma-separated list of domains that should not be proxied.
 	// Use `false` to disable proxies, ignoring environment variables.
  	// `auth` indicates that HTTP Basic auth should be used to connect to the proxy, and
  	// supplies credentials.
  	// This will set an `Proxy-Authorization` header, overwriting any existing
  	// `Proxy-Authorization` custom headers you have set using `headers`.
  	proxy: {
    	host: '127.0.0.1',
    	port: 9000,
    	auth: {
	      	username: 'mikeymike',
	      	password: 'rapunz3l'
    	}
  	},

  	// `cancelToken` specifies a cancel token that can be used to cancel the request
  	// (see Cancellation section below for details)
  	cancelToken: new CancelToken(function (cancel) {
  	})
}
~~~

### response Schema

+ 
~~~javascript
{
	// 服务器的响应数据
	data: {},

	// 响应状态码
	status: 200,

	// 状态码对应的 文本
	statusText: "OK",

	// 响应头  都是小写的
	headers: {},

	// 请求中为 axios 设置的 config
	config: {},

	// 产生这个响应的 request 
	request: {}
}
~~~

### config defaults

+ 为 config 参数设置默认值

+ 全局
~~~javascript
axios.defaults.baseURL = "",
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
~~~

+ 实例
~~~javascript
// Set config defaults when creating the instance
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// Alter defaults after instance has been created
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
~~~

### 拦截器

+ 拦截 request 或者 response 

### 使用 application/x-www-form-urlencoded

+ 默认状态下，axios 将 data 转换为 ```JSON``` 格式,若想使用 ```x-www-form-urlencoded``` 格式发送数据可以使用以下几种方式

	- URLSearchParams
	~~~javascript
	const params = new URLSearchParams()
	params.append('params1', 'value')
	params.append('params2', 'value')
	axios.post(url, params)
	// 但是不是所有的浏览器都支持 URLSearchParams
	~~~

	- 使用 ```qs``` 库
	~~~javascript
	import qs from "qs"
	axios.post(url, qs.stringify({params1: value}))

	// 或者使用以下方式
	import qs from 'qs';
	const data = { 'bar': 123 };
	const options = {
	  method: 'POST',
	  headers: { 'content-type': 'application/x-www-form-urlencoded' },
	  data: qs.stringify(data),
	  url,
	};
	axios(options);
	~~~

### 错误处理

+ catch

### cancel token

