---
sidebar_position: 2
---

## webpack基础
### 1.1 安装webpack
本地安装一下 webpack 以及 webpack-cli
```js
npm install webpack webpack-cli -D
```

### 1.2 工作模式
1. 新建 ./src/index.js 文件
```js
const a = 'Hello ITEM'
console.log(a)
module.exports = a;
```
2. 直接运行 npx webpack，启动打包
打包完成，我们看到日志上面有一段提示：The 'mode' option has not been set,...

|选项	| 描述 |
|------|------|
|development	|开发模式，打包更加快速，省了代码优化步骤|
|production	|生产模式，打包比较慢，会开启 tree-shaking 和 压缩代码|
|none	|不使用任何默认优化选项|

配置只需在配置对象中提供 mode 选项：
```js
module.exports = {
    mode: 'development',
};
```
复制代码
从 CLI 参数中传递：
```js
npx webpack --mode=development
```

### 1.3 配置文件

1. 根路径下新建一个配置文件 webpack.config.js

2. 新增基本配置信息
```js
const path = require('path')

module.exports = {
    mode: 'development', // 模式
    entry: './src/index.js', // 打包入口地址
    output: {
        filename: 'bundle.js', // 输出文件名
        path: path.join(__dirname, 'dist') // 输出文件目录
    }
}
```

### 1.4 loader
1. 新增 ./src/main.css
```css
body {
    margin: 0 auto;
    padding: 0 20px;
    max-width: 800px;
    background: #f4f8fb;
}
```
2. 修改 entry 配置
```js
pnpm install css-loader -D
```

3. 安装css-loader来处理CSS
```js
const path = require('path')

module.exports = {
    mode: 'development', // 模式
    entry: './src/main.css', // 打包入口地址
    output: {
        filename: 'bundle.css', // 输出文件名
        path: path.join(__dirname, 'dist') // 输出文件目录
    }
}
```

结论：loader 可以将 Webpack 转化js,css代码

### 1.5插件
1. 新建 ./src/index.html 文件，并添加代码

2. 本地安装 html-webpack-plugin
```js
pnpm install html-webpack-plugin -D
```
3. 配置插件
```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    ...
    plugins:[ // 配置插件
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ]
}
```

### 1.6自动清理打包目录
1. 安装清包插件
```js
pnpm install clean-webpack-plugin -D
```
2. 配置
```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 引入插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
    ...
    plugins:[ // 配置插件
        new HtmlWebpackPlugin({
            template: './src/index.html'
        }),
        new CleanWebpackPlugin() // 引入清包插件
    ]
}
```

### 1.7 区分环境
本地开发和部署线上，肯定是有不同的需求
本地环境：

需要更快的构建速度
需要打印 debug 信息
需要 live reload 或 hot reload 功能
需要 sourcemap 方便定位问题
...

生产环境：

需要更小的包体积，代码压缩+tree-shaking
需要进行代码分割
需要压缩图片体积
...

针对不同的需求，首先要做的就是做好环境的区分

1. 本地安装 cross-env 
```js
pnpm install cross-env -D
```
2. 配置启动命令
```js
"scripts": {
    "dev": "cross-env NODE_ENV=dev webpack serve --mode development", 
    "test": "cross-env NODE_ENV=test webpack --mode production",
    "build": "cross-env NODE_ENV=prod webpack --mode production"
},
```
3. 在 Webpack 配置文件中获取环境变量
```js
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

console.log('process.env.NODE_ENV=', process.env.NODE_ENV) // 打印环境变量

const config = {
    mode: "development", 
    entry: "./src/index.js", 
    output: {
        filename: "bundle.js", 
        // path: path.join(__dirname, "development"),
        // path: path.join(__dirname, "production"),
        path: path.join(__dirname, "dist"),
    },
    module: { 
        rules: [ // 转换规则
            {
                test: /\.css$/, //匹配所有的 css 文件
                use: 'css-loader' // use: 对应的 Loader 名称
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: './src/index.html'
        }),
        new CleanWebpackPlugin(), // 引入清包插件
    ]
};

module.exports = (env, argv) => {
    console.log('argv.mode=',argv.mode) // 打印 mode(模式) 值
    // 这里可以通过不同的模式修改 config 配置
    return config;
}
```
4. 测试一下看看

执行 npm run build
```js
process.env.NODE_ENV= prod
argv.mode= production
```

执行 npm run test
```js
process.env.NODE_ENV= test
argv.mode= production
```

执行 npm run dev
```js
process.env.NODE_ENV= dev
argv.mode= development
```

### 1.8 启动 devServer
1. 安装 webpack-dev-server
```js
pnpm intall webpack-dev-server@3.11.2 -D
```
⚠️注意：本文使用的 webpack-dev-server 版本是 3.11.2，当版本 version >= 4.0.0 时，需要使用 devServer.static 进行配置，不再有 devServer.contentBase 配置项。

2. 配置本地服务
```js
// webpack.config.js
const config = {
    ...
    devServer: {
        contentBase: path.resolve(__dirname, 'public'), // 静态文件目录
        compress: true, //是否启动压缩 gzip
        port: 8080, // 端口号
        // open:true  // 是否自动打开浏览器
    },
    // ...
}
module.exports = (env, argv) => {
    console.log('argv.mode=',argv.mode) // 打印 mode(模式) 值
    // 这里可以通过不同的模式修改 config 配置
    return config;
}
```

为什么要配置 contentBase ？
因为 webpack 在进行打包的时候，对静态文件的处理，例如图片，都是直接 copy 到 dist 目录下面。但是对于本地开发来说，这个过程太费时，也没有必要，所以在设置 contentBase 之后，就直接到对应的静态目录下面去读取文件，而不需对文件做任何移动，节省了时间和性能开销。

3. 启动本地服务
```js
pnpm run dev
```
http://localhost:9000/ 后添加 /bee.jpeg

### 1.9 引入CSS
1. 安装 style-loader
```js
pnpm install style-loader -D
```
2. 配置 loader
```js
const config = {
    ...
    module: { 
        rules: [{
            test: /\.css$/, //匹配所有的 css 文件
            use: ['style-loader','css-loader']
        }]
    },
    ...
}
```
⚠️注意： Loader 的执行顺序是固定从后往前，即按 css-loader --> style-loader 的顺序执行
3. 引入样式文件
```js
// ./src/index.js

import './main.css';


const a = 'Hello ITEM'
console.log(a)
module.exports = a;
```
```css
body {
    margin: 0 auto;
    padding: 0 20px;
    max-width: 800px;
    background: url('../public/bee.jpeg') no-repeat;
    object-fit: cover;
}
```
4. 重启本地服务，样式引入界面

### 1.10 CSS兼容性
用到的 transform: translateX(-50%);，需要加上不同的浏览器前缀，这个我们可以使用 postcss-loader 来帮助我们完成
```js
pnpm install postcss postcss-loader postcss-preset-env -D
```
```js
const config = {
    ...
    module: { 
    rules: [{
            test: /\.css$/, //匹配所有的 css 文件
            use: ['style-loader','css-loader', 'postcss-loader']
        }]
    },
    ...
}
```
创建 postcss 配置文件 postcss.config.js
```js
// postcss.config.js
module.exports = {
    plugins: [require('postcss-preset-env')]
}
```
创建 postcss-preset-env 配置文件 .browserslistrc [autoprefixer](http://autoprefixer.github.io/)
```js
# 换行相当于 and
last 2 versions # 回退两个浏览器版本
> 0.5% # 全球超过0.5%人使用的浏览器，可以通过 caniuse.com 查看不同浏览器不同版本占有率
IE 10 # 兼容IE 10
```

### 1.11 引入 Less 或者 Sass
less 和 sass 同样需要对应的loader来处理
| 文件类型	| loader |
|---|---|
|Less	|less-loader|
|Sass	|sass-loader node-sass 或 dart-sass|

Less 处理相对比较简单，直接添加对应的 Loader 就好

Sass 不光需要安装 sass-loader 还得搭配一个 node-sass，这里 node-sass 建议用淘宝镜像来安装，npm 安装成功的概率太小了 🤣

1. 安装
```js
pnpm install sass-loader node-sass -D
```
2. 新建 ./src/sass.scss
```js
$color: rgb(190, 23, 168);

body {
    p {
        background-color: $color;
        width: 300px;
        height: 300px;
        display: block;
        text-align: center;
        line-height: 300px;
    }
}
```
3. 引入 Sass 文件
```js
import './main.css';
import './sass.scss' // 引入 Sass 文件
const a = 'Hello ITEM'
console.log(a)
module.exports = a;
```

4. 修改配置
```js
const config = {
    ...
    rules: [{
        test: /\.(s[ac]|c)ss$/i, //匹配所有的 sass/scss/css 文件
        use: [
            'style-loader',
            'css-loader',
            'postcss-loader',
            'sass-loader', 
        ]},
    ]},
    ...
}
```

### 1.12 分离样式文件
1. 安装 mini-css-extract-plugin
```js
pnpm install mini-css-extract-plugin -D
```
2. 修改 webpack.config.js 配置
```js
// ...
// 引入插件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')


const config = {
    module: { 
        rules: [{
            test: /\.(s[ac]|c)ss$/i, //匹配所有的 sass/scss/css 文件
            use: [
          // 'style-loader',
          MiniCssExtractPlugin.loader, // 添加 loader
          'css-loader',
          'postcss-loader',
          'sass-loader', 
        ]},
    ]},
    plugins:[ // 配置插件
        new MiniCssExtractPlugin({ // 添加插件
            filename: '[name].[hash:8].css'
        }),
    ]
}
```
3. 查看打包结果

### 1.13 图片和字体文件
资源似乎默认引入了
[default mode](https://juejin.cn/post/7023242274876162084#heading-14)

# sdd