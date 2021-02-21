---

title: Axios
date: {{ date }}
updated: {{date}}
tags: Axios
categories: 工具

---
### Axios  基本入门

- Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

#### Axios 请求方式

- 基本请求

  > 使用请求方法函数
  >
  > axios.request(config)
  >
  > axios.get(url[, config])
  >
  > axios.delete(url[, config])
  >
  > axios.head(url[, config])
  >
  > axios.options(url[, config])
  >
  > axios.post(url[, data[, config]])
  >
  > axios.put(url[, data[, config]])
  >
  > axios.patch(url[, data[, config]])
  >
  > 
  >
  > 在使用方法时， url、method、data 这些属性都不必在配置中指定。 

- 并发请求

  > ```
  > axios.all([getUserAccount(), getUserPermissions()])
  > .then(axios.spread(function (res1, res2) {
  >  // 两个请求现在都执行完成
  > }));
  > //默认情况下 返回一个多个对象的数组, 如果想分开各个对象, 使用axios.spread() 函数 返回各个对象
  > ```

- 通过向 `axios` 传递相关配置来创建请求

  > ```
  > axios({
  >   method: 'post',
  >   url: '/user/12345',
  >   data: {
  >     firstName: 'Fred',
  >     lastName: 'Flintstone'
  >   }
  > });
  > ```

-   axios(url[, config])  :axios('/user/12345');

  > 发送 GET 请求（将使用默认的方法）





#### Axios 实例化

- 创建Axios 实例

  > 可以使用自定义配置新建一个 axios 实例
  >
  > ```
  > axios.create({config..})
  > ```

- 配置默认值

- 你可以指定用在各个请求配置的默认值

  > 全局的 axios 默认值 ::  axios.defaults.baseURL = 'https://api.example.com';
  >
  > 自定义实例默认值:: 
  >
  > const instance = axios.create({baseURL: 'https://api.example.com'});
  > instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;

  - 配置会以一个优先顺序进行合并 ,  默认值< 实例的defaultes属性 < 请求的config

#### 拦截器

- 在请求或响应被 `then` 或 `catch` 处理前拦截它们。

- ```
  // 添加请求拦截器
  axios.interceptors.request.use(function (config) {})
  // 添加响应拦截器
  axios.interceptors.response.use(function (response) {})
  ```

- 移除拦截器: axios.interceptors.request.eject(拦截器名字)



- 默认情况下，axios将JavaScript对象序列化为JSON。 要以application / x-www-form-urlencoded格式发送数据，需要更改格式