#   Vue项目

## 脚手架的搭建

### Yarn

- 速度快
- 离线模式（在缓存中存在）
- 更加简洁的输出
- 安装的版本统一（Yarn）

```yacas
yarn add react
npm install --global yarn
```

### npm

```shell
npm insatll vue
```

### node.js安装与下载

```shell
node -v //查看node环境
npm  -v //查看npm的版本
```

![image-20221006210235345](E:\Typora\data\img\image-20221006210235345.png)

### 安装cnmp

```
npm install -g cnpm --register=https://registry.npmmirror.com
```

![image-20221006210703677](E:\Typora\data\img\image-20221006210703677.png)

#### 使用cnpm安装vue-cli

```
cnpm install -g @vue/cli   
```

#### 检查是否安装成功

```
vue -V
```

![image-20221006211211729](E:\Typora\data\img\image-20221006211211729.png)

### 创建脚手架工程

> 项目名不能有大写

```
vue create 项目名


```

### 运行vue-cli

```
npm run serve
```

## 关闭eslint校验

> 在vue.config.js文件中配置

```js
  lintOnSave:false  //关闭es校验
```



## element-ui的引入

### 在线引入

```
.........
```

### 全局引入

#### 下载

```shell
npm i element-ui -S
```

#### 全局配置

##### 在main.js中配置

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
```

##### 报错

```vue
'ElementUI' is defined but never used  no-unused-vars
```

``` json
  "rules": {
    "generator-star-spacing": "off",
    "no-tabs":"off",
    "no-unused-vars":"off",
    "no-console":"off",
    "no-irregular-whitespace":"off",
    "no-debugger": "off"
}
}
```

### 项目打包

```shell
npm run build
```

## Vue路由的使用



### 下载vue-router

```shell
npm i vue-router@3.2.0
```

### 建立路由index.js

```js
import Vue from 'vue'
import Router from 'vue-router'
import Home from '../components/Home.vue'
import User from '../components/User.vue'
import Main from '../components/Main.vue'
Vue.use(Router)
// 1.创建路由
// 2. 将路由与组件进行映射
// 3.创建router实例

export default new Router({
    routes: [
      //这里是主页路由
      {
        path:'/',
        name:'Main',
        component:Main,
        children:[
            {
                path: '/home',
                name: 'Home',
                component: Home,
              },{
                path: '/user',
                name: 'User',
                component: User,
              }
        ]
      },
      {
        path: '/home',
        name: 'Home',
        component: Home,
      },{
        path: '/user',
        name: 'User',
        component: User,
      }
    ]
})
   
```

### 在main.js中

```js
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import axios from "axios";
// import axios from './utils/request.js';
axios.defaults.baseURL='http://127.0.0.1:8000/ajax'
//将cookie导入全局
// import { setCookie, getCookie, checkCookie, clearCookie } from 'utils/cookie';
Vue.config.productionTip = false
Vue.prototype.$axios=axios
Vue.use(ElementUI);
/* eslint-disable no-new */
new Vue({
  el: '#app',
  beforeCreate() {
    Vue.prototype.$bus = this //全局事件总线
  },
  //配置路由
  router,
  components: {App},
  template: '<App/>'
})

```



## 左侧菜单的实现



```js
<template>
  <el-row class="tac">
    <el-col :span="12">
        <h5>通用后台管理</h5>
        <el-menu
      mode="vertical"
      default-active="2"
      class="el-menu-vertical-demo"
      @open="handleOpen"
      @close="handleClose">
      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>导航一</span>
        </template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="1-1">选项1</el-menu-item>
          <el-menu-item index="1-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="1-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="1-4">
          <template slot="title">选项4</template>
          <el-menu-item index="1-4-1">选项1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-menu-item index="2">
        <i class="el-icon-menu"></i>
        <span slot="title">导航二</span>
      </el-menu-item>
      <el-menu-item index="3" disabled>
        <i class="el-icon-document"></i>
        <span slot="title">导航三</span>
      </el-menu-item>
      <el-menu-item index="4">
        <i class="el-icon-setting"></i>
        <span slot="title">导航四</span>
      </el-menu-item>
    </el-menu>
    </el-col>
  </el-row>
</template>

<script>
export default {
    name:"CommonAsider",
    data() {
        return {
            isCollapse:false
        }
    }
}
</script>
<style>

</style>
```

# Axios在Vue中的使用

> axios是基于Promise的，因此可以使用Promise API
>

## axios的请求方式：

> axios(config)
> axios.request(config)
> axios.get(url [,config])
> axios.post(url [,data [,config]])
> axios.put(url [,data [,config]])
> axios.delete(url [,config])
> axios.patch(url [,data [,config]])
> axios.head(url [,config])
>

```js
//执行GET请求
import axios from 'axios'
axios.default.baseURL = 'http://localhost:3000/api/products'
axios.get('/user?ID=12345')  //返回的是一个Promise
    .then(res=>console.log(res))
    .catch(err=>console.log(err));

//可配置参数的方式
axios.get('/user',{
    params:{
        ID:12345
    }
}).then(res=>console.log(res))
    .catch(err=>console.log(err));

//发送post请求
axios.post('/user',{
    firstName: 'simon',
    lastName:'li'
}).then(res=>console.log(res))
    .catch(err=>console.log(err));
```

## 发送并发请求

> 通过axios.all(iterable)可实现发送多个请求，参数不一定是数组，只要有iterable接口就行，
>
> 函数返回的是一个数组axios.spread(callback)可用于将结果数组展开

```js
//发送多个请求(并发请求)，类似于promise.all，若一个请求出错，那就会停止请求
const get1 = axios.get('/user/12345');
const get2 = axios.get('/user/12345/permission');
axios.all([get1,get2])
    .then(axios.spread((res1,res2)=>{
    	console.log(res1,res2);
	}))
    .catch(err=>console.log(err))

```

