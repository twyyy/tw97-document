# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

开发工具：vscode

Node.js版本：16.15.1

本文档演示过程中所使用的js包管理工具为yarn2，如果您对它不熟悉，可以前往<https://www.yarnpkg.cn/>了解它如何安装以及它的基本用法，本文档不做说明。

当然，如果您更熟悉或喜欢使用其他包管理工具，您也可以按照您自己的方式来管理。

------

## 创建html+scss+ts文件

注：创建文件的路径和命名按自己习惯或公司规范即可，无需雷同。

本次演示创建的文件及目录结构如下：

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

本次演示安装完成后目录结构如下（由于node_modules目录结构复杂，所以从此处开始及后续的目录结构均不展示）：

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

本次演示创建后的目录结构如下：

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

## 安装css-loader、sass、sass-loader

```shell
yarn add css-loader sass sass-loader -D
```

|名称|说明|版本|
|----|----|----|
|css-loader|css-loader 会对 @import 和 url() 进行处理，就像 js 解析 import/require() 一样。（内容来自于<https://webpack.docschina.org/loaders/css-loader/>）|^6.7.1|
|sass|sass文件编译器|^1.53.0|
|sass-loader|加载 Sass/SCSS 文件并将他们编译为 CSS。（内容来自于：<https://webpack.docschina.org/loaders/sass-loader/>）|^13.0.2|

webpack.config.js需要加入以下内容：

```javascript
// 省略已配置的内容……

module.exports = {
  // 省略已配置的内容……

  // loader
  module: {
    // 解析规则配置
    rules: [
      {
        test: /\.scss$/, // 匹配文件名称的正则表达式
        use: ['css-loader', 'sass-loader'] // loader顺序从右往左，从下往上
      }
    ]
  }
};
```

------

## 安装style-loader或mini-css-extract-plugin

本次演示选择使用mini-css-extract-plugin插件。请自行决定使用哪种方式或两者皆使用。

|名称|说明|版本|
|----|----|----|
|style-loader|把 CSS 插入到 DOM 中。（内容来自于<https://webpack.docschina.org/loaders/style-loader/>）|^3.3.1|
|mini-css-extract-plugin|本插件会将 CSS 提取到单独的文件中，为每个包含 CSS 的 JS 文件创建一个 CSS 文件，并且支持 CSS 和 SourceMaps 的按需加载。（内容来自于<https://webpack.docschina.org/plugins/mini-css-extract-plugin/>）|^2.6.1|

### 安装style-loader

```shell
yarn add style-loader -D
```

webpack.config.js需要修改以下内容：

```javascript
// 省略已配置的内容……

module.exports = {
  // 省略已配置的内容……

  // loader
  module: {
    // 解析规则配置
    rules: [
      {
        test: /\.scss$/, // 匹配文件名称的正则表达式
        use: ['style-loader','css-loader', 'sass-loader'] // loader顺序从右往左，从下往上
      }
    ]
  }
};
```

### 安装mini-css-extract-plugin

```shell
yarn add mini-css-extract-plugin -D
```

webpack.config.js需要修改以下内容：

```javascript
// 省略已配置的内容……

// 引入mini-css-extract-plugin插件
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  // 省略已配置的内容……

  // 插件
  plugins: [
    // 省略已配置的内容……

    // 分离CSS
    new MiniCssExtractPlugin({
      // 输出文件名（相对路径于output.path）[name]表示原文件名不含后缀，[fullhash]表示哈希值
      filename: './css/[name].[fullhash].css'
    })
  ],

  // loader
  module: {
    // 解析规则配置
    rules: [
      {
        test: /\.scss$/, // 匹配文件名称的正则表达式
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader'] // loader顺序从右往左，从下往上
      }
    ]
  }
};
```

------

## 打包

配置到此，项目已经可以打包使用了

package.json文件增加以下内容：

```json
{
  // 省略内容……

  "scripts": {
    // --config 后面就是webpack配置文件的相对路径
    "build": "webpack --config ./build/webpack.config.js"
  }
}
```

使用以下命令打包：

```shell
yarn run build
```

命令执行完成后，在您webpack.config.js文件中配置的output.path路径应该已经生成了打包后的文件。

本次演示打包后目录结构如下（如果使用style-loader，目录结构中将不会存在css文件夹及文件，因为css样式已经全部打包在了js文件中）：

```shell
test
├── build
│   └── webpack.config.js
├── dist
│   ├── css
│   │   ├── login.08d234ab4ec7baea2bba.css
│   │   └── register.08d234ab4ec7baea2bba.css
│   ├── js
│   │   ├── login.08d234ab4ec7baea2bba.js
│   │   └── register.08d234ab4ec7baea2bba.js
│   ├── login.html
│   └── register.html
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

## 打包图片、字体等其他媒体类型的文件

在src目录中又新建立了两个文件夹，一个叫做fonts，用来放置字体文件。一个叫做images，用来放置图片文件。

随便选择一张图片和一个字体放入对应文件夹进行测试

在login.scss中引用字体和图片，修改后如下：

```scss
@font-face {
  font-family: "包图小白体";
  src: url("../fonts/包图小白体.ttf");
}

html,
body {
  padding: 0;
  margin: 0;
  // background-color: green;
  background-image: url("../images/test.png");
  font-family: "包图小白体";
}
```

此时如果不做任何配置直接打包，图片文件和字体文件将会直接打包在webpack配置中output.path设置的目录下，且文件名格式为[哈希值].[扩展名]。通常，我希望把每一种文件归类放置。所以接下来，我需要修改webpack配置文件，将图片和字体文件归类放置，其他媒体类型文件配置方式也大致相同。如果您有其他的打包策略，可以参考<https://webpack.docschina.org/guides/asset-modules/#resource-assets>选择您需要的方式

webpack.config.js添加以下配置：

```javascript
// 省略已配置的内容……

module.exports = {
  // 省略已配置的内容……

  // loader
  module: {
    rules: [
      // 省略已配置的内容……

      {
        test: /\.ttf$/, // 匹配扩展名为ttf的文件
        type: 'asset/resource', // 发送一个单独的文件并导出 URL。
        generator: {
          // 输出文件名（相对路径于output.path）[name]表示原文件名，[hash]表示哈希值，[ext]表示.扩展名，
          filename: './fonts/[name].[hash][ext]'
        }
      },
      {
        test: /\.png$/,
        type: 'asset/resource',
        generator: {
          filename: './images/[name].[hash][ext]'
        }
      }
    ]
  }
};
```

修改完webpack配置文件后再次执行打包。这时资源文件的生成方式就按照配置执行了

------

## 安装webpack-dev-server

```shell
yarn add webpack-dev-server -D
```

|名称|说明|版本|
|----|----|----|
|webpack-dev-server|可用于快速开发应用程序。（内容来自于<https://webpack.docschina.org/configuration/dev-server/>）|^4.9.3|

如果以现在的方式来进行开发，修改一个文件并且重新打包之后才能看到效果，那一定会令人崩溃的。

而webpack-dev-server热加载，就很好的解决了这个问题。

以下内容说明来自于<https://webpack.docschina.org/configuration/dev-server/>

webpack.config.js添加以下内容：

```javascript
// 省略已配置的内容……

module.exports = {
  // 省略已配置的内容……

  // 开发服务器配置
  devServer: {
    static: [
      {
        // 告诉服务器从哪里提供内容。
        directory: path.resolve(__dirname, '../dist')
      }
    ],
    // 指定要使用的 host。
    host: '127.0.0.1',
    // 指定监听请求的端口号
    port: 8080,
    // 启用热加载
    hot: true,
    // 告诉 dev-server 在服务器已经启动后打开浏览器。
    open: true
  }
};
```

package.json文件增加以下内容：

```json
{
  // 省略内容……

  "scripts": {
    // 省略内容……

    // --config 后面就是webpack配置文件的相对路径
    "start": "webpack serve --config ./build/webpack.config.js"
  }
}
```

使用以下命令启动开发服务器：

```shell
yarn run start
```

启动后通过host和port就可以在浏览器访问您的项目。

但是，如果您使用的是与我演示中相同的配置，您就会发现有以下问题。

------

### 每次打开或刷新页面都会有警告信息

这是因为没有在webpack配置文件中或者start命令中指定模式

webpack会将mode自动设置为production（生产）模式，导致问题发生

解决方法：

在webpack.config.js配置中添加mode配置（不建议，因为会影响build命令）：

```javascript
// 省略已配置的内容……

module.exports = {
  // 省略已配置的内容……

  // 开发模式
  mode:'development'
};
```

或者

修改package.json文件start命令如下：

```json
{
  // 省略内容……

  "scripts": {
    // 省略内容……

    // --config 后面就是webpack配置文件的相对路径 --mode 后面表示开发模式
    "start": "webpack serve --config ./build/webpack.config.js --mode development"
  }
}
```

------

### 修改html文件并保存后不会自动刷新页面

------
