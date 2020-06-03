## 手把手搭建一个vue项目

1. 创建一个项目
`vue create vue-app`

2. 双页面布局
```
> default.vue  基础页面模板
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




