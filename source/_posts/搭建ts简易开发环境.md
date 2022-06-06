---
title: 搭建ts简易开发环境
date: 2021-06-01 14:09:55
tags:
- Webpack
- 原创
- TypeScript
categories:
- 前端

---
用webpack搭建一个用于学习ts的简易的ts开发环境...
<!--more-->
#### 初始化项目

```code
mkdir ts-cli
cd ts-cli
npm init -y
```

#### 下载typescript依赖

```
npm i typescript -D
```

#### 初始化typescript配置项

```
npx tsc --init
```

初始化配置成功会提示如下

![image-20210601133709223](https://fastly.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210601133709223.png)

我们这里搭建的是便于学习ts的开发环境，我们再添加一些目录文件，index.html，和webpack配置文件build/webpack.config.js src/main.ts、tslint.json...

代码结构如下

```pseudocode
ts-cli
└───build
│   │   webpack.config.js
└───index.html
└───node_modules
│   │
└─── package.json
└───src
│   │   main.ts
└─── tsconfig.json
└─── tslint.json
└─── REAME.md
```

#### 下载webpack依赖

```
"cross-env": "^6.0.3",
"html-webpack-plugin": "^3.2.0",
"ts-loader": "^6.2.1",
"webpack": "^4.41.4",
"webpack-cli": "^3.3.10",
"webpack-dev-server": "^3.10.1"
```

#### webpack.config.js

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/main.ts",
  output: {
    filename: "build.js"
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"]
  },
  module: {
    rules: [
      {
        test: /.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/
      }
    ]
  },
  devtool: process.env.NODE_ENV === "production" ? false : "inline-source-map",
  devServer: {
    contentBase: "./dist",
    stats: "errors-only",
    compress: false,
    host: "localhost",
    port: 8080
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./index.html"
    })
  ]
};
```

比较简单的webpack配置，设置好入口和出口文件，然后用插件HtmlWebpackPlugin插件把build.js引入到index.html里...
