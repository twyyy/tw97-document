# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

## 引入依赖包

```shell
yarn add webpack webpack-cli webpack-dev-server html-webpack-plugin mini-css-extract-plugin style-loader css-loader sass sass-loader typescript ts-node postcss-loader autoprefixer -D
```

|包名|说明|参考|
|-|-|-|
|webpack|打包工具|<https://webpack.docschina.org/concepts/>|
|webpack-cli|通过命令向webpack配置传参|<https://webpack.docschina.org/api/cli/>|
|webpack-dev-server|开发服务器，热加载|<https://webpack.docschina.org/configuration/dev-server/>|
|html-webpack-plugin|打包html，自动引入js、css等|<https://webpack.docschina.org/plugins/html-webpack-plugin/>|
|mini-css-extract-plugin|将打包到js的css代码提出|<https://webpack.docschina.org/plugins/mini-css-extract-plugin/>|
|style-loader|将css打包到js中|<https://webpack.docschina.org/loaders/style-loader/>|
|css-loader|解析css代码中的外部引入|<https://webpack.docschina.org/loaders/css-loader/>|
|sass|sass文件编译器|<https://sass-lang.com/>|
|sass-loader|将sass、scss编译为css|<https://webpack.docschina.org/loaders/sass-loader/>|
|typescript|js的超集|<https://www.tslang.cn/>|
|ts-node|运行webpack ts配置|<https://www.npmjs.com/package/ts-node>|
|postcss-loader|配合autoprefixer插件|<https://webpack.docschina.org/loaders/postcss-loader/>|
|autoprefixer|自动添加css浏览器前缀|<https://www.npmjs.com/package/autoprefixer>|

## webpack配置

### webpack.config.ts

```typescript
import path from 'path'; // node.js处理路径的工具

import { WebpackPluginInstance, Configuration, RuleSetRule } from 'webpack'; // 接口
import webpackDevServer from 'webpack-dev-server'; // 开发服务器

import HtmlWebpackPlugin from "html-webpack-plugin"; // html插件
import MiniCssExtractPlugin from "mini-css-extract-plugin"; // 分离css插件
import AutoPrefixer from "autoprefixer"; // css浏览器前缀插件

const tsPath = path.resolve(__dirname, './src/ts'); // ts文件目录
const outputPath = path.resolve(__dirname, './dist'); // 输出目录
const viewPath = path.resolve(__dirname, './src/views'); // html文件目录

// 输出配置
const config: Configuration = {};

// 模式
const isProductionMode = process.env.NODE_ENV?.trim() === "production";
config.mode = isProductionMode ? 'production' : 'development';

// 入口
config.entry = {
  login: path.resolve(tsPath, 'login.ts'),
  register: path.resolve(tsPath, 'register.ts')
};

// 输出
config.output = {
  path: outputPath,
  filename: `./js/[name]${isProductionMode ? ".[fullhash]" : ""}.js`,
  clean: true
};

// 加载器
const rules: RuleSetRule[] = [];
config.module = { rules };

// scss文件加载
rules.push({
  test: /\.scss$/,
  use: [
    isProductionMode ? MiniCssExtractPlugin.loader : 'style-loader',
    'css-loader',
    {
      loader: 'postcss-loader',
      options: {
        postcssOptions: {
          plugins: [
            AutoPrefixer
          ]
        }
      }
    },
    'sass-loader'
  ]
});

// ttf文件加载
rules.push({
  test: /\.ttf$/,
  type: 'asset/resource',
  generator: {
    filename: `./fonts/[name]${isProductionMode ? ".[hash]" : ""}[ext]`
  }
});

// png文件加载
rules.push({
  test: /\.png$/,
  type: 'asset/resource',
  generator: {
    filename: `./images/[name]${isProductionMode ? ".[hash]" : ""}[ext]`
  }
});

// 插件
const plugins: WebpackPluginInstance[] = [];
config.plugins = plugins;

// 生成登录页面
plugins.push(new HtmlWebpackPlugin({
  template: path.resolve(viewPath, 'login.html'),
  filename: 'login.html',
  chunks: ['login']
}));

// 生成注册页面
plugins.push(new HtmlWebpackPlugin({
  template: path.resolve(viewPath, 'register.html'),
  filename: 'register.html',
  chunks: ['register']
}));

// 分离css
if (isProductionMode) {
  plugins.push(new MiniCssExtractPlugin({
    filename: './css/[name].[fullhash].css'
  }));
}

// 开发服务器
if (!isProductionMode) {
  const devServer: webpackDevServer.Configuration =
  {
    static: [
      {
        directory: outputPath
      },
      {
        directory: viewPath // 让html文件也支持热加载
      }
    ],
    host: '127.0.0.1',
    port: 8080,
    hot: true,
    open: true
  };
  config.devServer = devServer;
}

export default config;
```

### 目录结构

忽略node_modules

```shell
test
├── package.json
├── src
│   ├── fonts
│   │   └── 包图小白体.ttf
│   ├── images
│   │   └── test.png
│   ├── scss
│   │   ├── login.scss
│   │   └── register.scss
│   ├── ts
│   │   ├── login.ts
│   │   └── register.ts
│   └── views
│       ├── login.html
│       └── register.html
├── webpack.config.ts
└── yarn.lock
```
