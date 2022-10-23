# Babel

> Babel 是一个用于 web 开发，且自由开源的 JavaScript 编译器、转换器。主要用于在当前和较旧的浏览器或环境中将 ECMAScript 2015+ 代码转换为 JavaScript 的向后兼容版本。
>
> Babel 使软件开发者能够以偏好的编程语言或风格来写作源代码，并将其利用 Babel 翻译成 JavaScript，是现今在浏览器最常用的编程语言。



## 下列是 Babel 的使用场景：

> 语法转换。
> 目标环境中缺少的 Polyfill 功能。
> 源代码转换（codemods）
> 示例：
> 例如将 ES2015 中的箭头函数编译成 ES5：

```js
[1, 2, 3].map((n) => n + 1);
1
编译后的 ES5 代码如下所示：

[1, 2, 3].map(function (n) {
  return n + 1;
});
```

## Babel的使用方法

```shell
npm install --global babel-cli

bable -- version  // 查看bable
```

![image-20221013205806252](E:\Typora\data\img\image-20221013205806252.png)

> 这两段代码的功能是一样的，但是因为 ES2015 和 ES5 的语法有所不同，所以编译后的代码也不同。
>
> Babel运行方式和插件
> Babel 的编译总共分为三个阶段：解析（parsing），转换（transformation），生成（generate）。
>
> Babel 本身不具有任何转化功能，Babel 的转换功能都是通过插件（plugin）来实现的，把转化的功能都分解到一个个插件里面。因此当我们不配置任何插件时，经过 Babel 的代码和输入是相同的。
>
> 插件总共分为两种：
>
> 语法插件：当我们添加语法插件之后，在解析这一步就使得 Babel 能够解析更多的语法。
> 转译插件：而添加转译插件之后，在转换这一步把源码转换并输出。这也是我们使用 Babel 最本质的需求。
> 同一类语法可能同时存在语法插件版本和转译插件版本。如果我们使用了转译插件，就不用再使用语法插件了。
>
> preset
>
> preset 预定义的一系列插件的组合，用于将特定的语法转换为当前环境使用的语法，避免了自己单独去挑选插件。

## preset 分为以下几种：

> 官方内容，目前包括 env、react、flow、minify 、typescript 等。
> stage-x，这里面包含的都是当年最新规范的草案，每年更新。可以细分为：
> Stage 0：设想（Strawman）：只是一个想法，可能有 Babel插件。
> Stage 1：建议（Proposal）：这是值得跟进的。
> Stage 2： 草案（Draft）：初始规范。
> Stage 3： 候选（Candidate）：完成规范并在浏览器上初步实现。
> Stage 4：完成（Finished）：将添加到下一个年度版本发布中。
>

## 使用步骤

### 初始化node工程

```
pip init -y
```

### 下载babel包

```
npm install --global babel-cli
```

### 配置相关文件

> babel的配置文件是.babelrc,存放在根目录下,该文件用来设置转码规则和插架,基本格式如下

```js
{
	"presets":[],
	"plugins":[]
}
```

```js
{
	"presets":["es2015"],
	"plugins":[]
}
```

### 安装es2015转码

```
npm install --save-dev babel-preset-es2015
```

![image-20221013213534303](E:\Typora\data\img\image-20221013213534303.png)

![image-20221013213600121](E:\Typora\data\img\image-20221013213600121.png)

### 使用命令转码

```
##将转码文件写入应该文件
##单个转码
babel 需转码的路径/需转码的文件.js --out-file 被转码的路径/被转码的文件

babel src/example.js -o dist/compiled.js
## 整个文件夹转码
babel src1 --out-flie dist2

babel src -d dist2

```

