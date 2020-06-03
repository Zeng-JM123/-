## 手把手搭建一个vue项目

1. 创建一个项目
`vue create vue-app`

2. 双页面布局
```
default.vue  基础页面模板
<template>
  <div class="main">
    <div class="nav">
      <Nav/>
    </div>
    <div class="content">
      <div class="left_content">
        <router-view/>
      </div>
      <right-content class="right_content"/>
    </div>
    <div class="footer">
      <Footer/>
    </div>
  </div>
</template>
```

3. 配置路由
```
router/index.js
const routes = [
  {
    path: '/',
    name: 'default',
    redirect: '/home',
    component: Default,
    children: [...]
  },
  {
    path: '/login',
    name: 'login',
    component: () => import('../views/login')
  }
]
```

4. 封装Axios
```
import axios from 'axios'
import Vue from 'vue'

axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'
var loading = '';

axios.defaults.baseURL = 'xxx';
Vue.prototype.baseURL = 'xxx';
// 请求拦截器
axios.interceptors.request.use(config => {
  console.log(config)
  loading =Vue.prototype.$loading({
    lock: true,
    text: '加载中.',
    spinner: 'el-icon-loading',
    background: 'rgba(0, 0, 0, 0.7)'
  });
  return config;
}, err => {
  return Promise.reject(err)
})

// 响应拦截器
axios.interceptors.response.use(response => {
  loading.close();
  return response;
}, err => {
  return Promise.reject(err)
})

// 封装axios的post请求
export function post(url, params){
  return new Promise((resolve, reject) => {
    axios.post(url, params).then(response => resolve(response.data)).catch(err => reject(err))
  })
}

// 封装axios的get请求
export function get(url, params= {}){
  return new Promise((resolve, reject) => {
    axios.get(url, {params: params}).then(response => resolve(response)).catch(err => reject(err))
  })
}

// 封装axios的delete请求
export function deletes(url, params){
  return new Promise((resolve, reject) => {
    axios.delete(url, {params: params}).then(response => resolve(response)).catch(err => reject(err))
  })
}

// 封装axios的put请求
export function put(url, params){
  return new Promise((resolve, reject) => {
    axios.put(url, params).then(response => resolve(response.data)).catch(err => reject(err))
  })
}


// console.log(Vue)
Vue.prototype.$post = post;
Vue.prototype.$get = get;
Vue.prototype.$deletes = deletes;
Vue.prototype.$put = put;
```



