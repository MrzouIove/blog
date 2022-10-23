# Webpack 入门教程

> Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

![img](https://www.runoob.com/wp-content/uploads/2017/01/32af52ff9594b121517ecdd932644da4.png)

## 安装 Webpack

> 在安装 Webpack 前，你本地环境需要支持 [node.js](https://www.runoob.com/nodejs/nodejs-install-setup.html)。
>
> 由于 npm 安装速度慢，本教程使用了淘宝的镜像及其命令 cnpm，安装使用介绍参照：[使用淘宝 NPM 镜像](https://www.runoob.com/nodejs/nodejs-npm.html)。
>
> 使用 cnpm 安装 webpack：

```shell
npm init -y //初始化

cnpm install webpack -g

npm install - g webpack webpack-cli

npm  uninstall  webpack  -g //卸载

npm  install  webpack@3.5.5 -g  --unsafe-perm //安装指定版本

```

## 使用webpack的步骤

### 接下来我们创建一个目录 webpackDemo：

```shell
mkdir webpackdemo
```

### 创建src文件夹

```js
exports.info = function(str){
  document.write(str)//浏览器输出
}
```

### 在src下创建common.js

```js
exports.info = function(a,b){
    return a+b
}
```

### 在src下创建utils.js

```js
const common = require('./common.js')
const utils = require('./utils.js')
common.info('hello'+utils.add(1,2))
```

### 在src中创建index.js

> 在main.js中引入上述文件中

```js
const common = require('./common.js')
const utils = require('./utils.js')
common.info('hello'+utils.add(1,2))
```

### 创建webpack的配置文件

```js
const path = require("path"); //Node.js内置模块
module.exports = {
    entry: "./src/index.js",//配置文件入口
    output: {
        path: path.resolve(__dirname,'./dist'),//输出路径,_dirname:当前文件所在的路径
        filename: "bundle.js"//输出文件
    }
	
};
```

## 命令执行打包操作

```shell
webpack

webpack --mode=development  ##没有报错
```

### 注意: webpack4版需要去额外安装`webpack-cli`

```kotlin
npm install webpack@4 --save-dev
```