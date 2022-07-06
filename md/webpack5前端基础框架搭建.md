# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

开发工具：vscode

Node.js版本：16.15.1

本文档所使用的js包管理工具为yarn2，如果您对它不熟悉，可以前往<https://www.yarnpkg.cn/>了解它如何安装以及它的基本用法，本文档不做说明。

当然，如果您更熟悉或喜欢使用其他包管理工具，您也可以按照您自己的方式来管理。

------

## 创建html+scss+ts文件

注：创建文件的路径和命名按自己习惯或公司规范即可，无需雷同。

本次创建的文件及目录结构如下：

```shell
test
└── src
    ├── login.html
    ├── register.html
    ├── scss
    │   ├── login.scss
    │   └── register.scss
    └── ts
        ├── login.ts
        └── register.ts
```

html代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- login文件此处为“登录”，register文件此处为“注册” -->
  <title>登录</title>
</head>

<body>
  <!-- login文件此处为“登录”，register文件此处为“注册” -->
  <h1>登录</h1>
</body>

</html>
```

scss代码如下：

```scss
html,
body {
  margin: 0;
  padding: 0;
  background-color: yellow; // login文件此处为“yellow”，register文件此处为“blue”
}
```

ts代码如下：

```typescript
// login文件此处为“login.scss”，register文件此处为“register.scss”
import "../scss/login.scss";
// login文件此处为“登录”，register文件此处为“注册”
console.log("登录");
```

------

## 安装webpack和webpack-cli包

注：从此处开始包括后续的所有命令都需要在您的项目根目录下执行

```shell
yarn add webpack webpack-cli -D
```

|名称|说明|版本|
|----|----|----|
|webpack|本质上，webpack 是一个用于现代 JavaScript 应用程序的 静态模块打包工具。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图(dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。（内容来自于<https://webpack.docschina.org/concepts/>）|^5.73.0|
|webpack-cli|为了更合适且方便地使用配置，可以在 webpack.config.js 中对 webpack 进行配置。CLI 中传入的任何参数会在配置文件中映射为对应的参数。（内容来自于<https://webpack.docschina.org/api/cli/>）|^4.10.0|

安装完成后项目根目录下会多出一个目录“node_modules”和两个文件“package.json”，“yarn.lock”

其中“node_modules”就是用来存放包的文件夹。

“package.json”是项目清单

“yarn.lock”锁定包的版本号，其他人在install时保证包版本的一致性

目录结构如下（由于node_modules目录结构复杂，所以从此处开始及后续的目录结构均不展示）：

```shell
test
├── package.json
├── src
│   ├── login.html
│   ├── register.html
│   ├── scss
│   │   ├── login.scss
│   │   └── register.scss
│   └── ts
│       ├── login.ts
│       └── register.ts
└── yarn.lock
```

------

## 创建webpack配置文件

注：创建文件的路径和命名按自己习惯或公司规范即可，无需雷同。

本次创建后的文件及目录结构如下：

```shell
test
├── build
│   └── webpack.config.js
├── package.json
├── src
│   ├── login.html
│   ├── register.html
│   ├── scss
│   │   ├── login.scss
│   │   └── register.scss
│   └── ts
│       ├── login.ts
│       └── register.ts
└── yarn.lock
```

webpack.config.js代码如下：

```javascript
// 当前文件的绝对路径
const path = require('path');

// 导出数据
module.exports = {

  // 入口
  entry: {
    login: path.resolve(__dirname, '../src/ts/login.ts'),
    register: path.resolve(__dirname, '../src/ts/register.ts')
  },

  // 输出
  output: {
    // 项目打包输出路径
    path: path.resolve(__dirname, '../dist'), // 项目打包输出路径
    // 输出文件名（相对路径于output.path）[name]表示原文件名不含后缀，[fullhash]表示哈希值
    filename: './js/[name].[fullhash].js',
    // 输出前删除现有文件
    clean: true
  }

};
```

------

## 安装html-webpack-plugin插件

```shell
yarn add html-webpack-plugin -D
```

|名称|说明|版本|
|----|----|----|
|html-webpack-plugin|简化了 HTML 文件的创建，以便为你的 webpack 包提供服务。这对于那些文件名中包含哈希值，并且哈希值会随着每次编译而改变的 webpack 包特别有用。你可以让该插件为你生成一个 HTML 文件，使用 lodash 模板提供模板，或者使用你自己的 loader。（内容来自于<https://webpack.docschina.org/plugins/html-webpack-plugin/>）|^5.5.0|

webpack.config.js需要加入以下内容：

```javascript
// 省略已配置的内容……

// 引入html-webpack-plugin插件
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // 省略已配置的内容……

  // 插件
  plugins: [
    // 生成登录页面
    new HtmlWebpackPlugin({
      // 页面模板
      template: path.resolve(__dirname, '../src/login.html'),
      // 生成的文件名（相对路径于output.path）
      filename: 'login.html',
      // 入口
      chunks: ['login']
    }),

    // 生成注册页面
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../src/register.html'),
      filename: 'register.html',
      chunks: ['register']
    })
  ]
};
```

------

## 安装css-loader、node-sass、sass-loader

```shell
yarn add css-loader node-sass sass-loader -D
```

|名称|说明|版本|
|----|----|----|
|css-loader|css-loader 会对 @import 和 url() 进行处理，就像 js 解析 import/require() 一样。（内容来自于<https://webpack.docschina.org/loaders/css-loader/>）|^6.7.1|
|node-sass||^7.0.1|
|sass-loader||^13.0.2|

------
