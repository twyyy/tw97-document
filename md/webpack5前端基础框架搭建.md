# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

开发工具：vscode

Node.js版本：16.15.1

# webpack及webpack-cli安装

```shell
npm install webpack webpack-cli -save-dev
```

本次安装的webpack版本为：^5.73.0

本次安装的webpack-cli版本为：^4.10.0

# 创建html+scss+ts文件

注：创建文件的路径和命名按自己习惯或公司标准即可，无需雷同。

本次创建的html路径如下：

```shell
./src/login.html
./src/register.html
```

本次创建的html代码如下：

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

本次创建的scss路径如下：

```shell
./src/scss/login.scss
./src/scss/register.scss
```

本次创建的scss代码如下：

```scss
html,
body {
  margin: 0;
  padding: 0;
  background-color: yellow; // login文件此处为“yellow”，register文件此处为“blue”
}
```

本次创建的ts路径如下：

```shell
./src/ts/login.ts
./src/ts/register.ts
```

本次创建的ts代码如下：

```typescript
// login文件此处为“login.scss”，register文件此处为“register.scss”
import "../scss/login.scss";
// login文件此处为“登录”，register文件此处为“注册”
console.log("登录");
```

# 安装loader、插件、开发服务器

```shell
npm install html-webpack-plugin css-loader node-sass sass-loader postcss-loader autoprefixer mini-css-extract-plugin css-minimizer-webpack-plugin webpack-dev-server --save-dev
```
