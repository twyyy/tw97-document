# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

## 引入依赖包

```shell
yarn add webpack webpack-cli webpack-dev-server html-webpack-plugin mini-css-extract-plugin style-loader css-loader sass sass-loader typescript ts-node raw-loader -D
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

## webpack配置
