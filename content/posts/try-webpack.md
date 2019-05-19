---
title: "Try Webpack"
date: 2016-07-08T10:25:13+08:00
draft: false
tags: ["webpack"]
keywords: []
description: ""
slug: ""
---

前不久把 sf 前端的构建工具进行了改进和优化，用上了目前非常火的 webpack 、babel 和 es6 等等新技术。

### 历史

sf 前端的构建工具最早使用的是当时非常流行的 grunt，接下来是 gulp，然后就是现在的 webpack。

### 构建工具比较

| 构建工具 | Browserify                                                                                                                                                                      | Grunt                                                                                                                             | Gulp                                                                                                                         | Webpack                                                                                                                                                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 描述     | browser-side require() the node way                                                                                                                                             | The JavaScript Task Runner                                                                                                        | The streaming build system                                                                                                   | Packs CommonJs/AMD modules for the browser. Allows to split your codebase into multiple bundles, which can be loaded on demand. Support loaders to preprocess files, i.e. json, jade, coffee, css, less, ... and your custom stuff. |
| 关键词   | browser, require, commonjs, commonj-esque, bundle, npm, javascript                                                                                                              | task, async, cli, minify, uglify, build, lodash, unit, test, qunit, nodeunit, server, init, scaffold, make, jake, tool            |                                                                                                                              |                                                                                                                                                                                                                                     |
| 作者     | James Halliday                                                                                                                                                                  | Grunt Development Team                                                                                                            | Fractal                                                                                                                      | Tobias Koppers @sokra                                                                                                                                                                                                               |
| 链接     | [ Homepage](https://github.com/substack/node-browserify)[ Bug Report](https://github.com/substack/node-browserify/issues)[ Github](https://github.com/substack/node-browserify) | [ Homepage](http://gruntjs.com/)[ Bug Report](https://github.com/gruntjs/grunt/issues)[ Github](https://github.com/gruntjs/grunt) | [ Homepage](http://gulpjs.com/)[ Bug Report](https://github.com/gulpjs/gulp/issues)[ Github](https://github.com/gulpjs/gulp) | [ Homepage](https://github.com/webpack/webpack)[ Bug Report](https://github.com/webpack/webpack/issues)[ Github](https://github.com/webpack/webpack)                                                                                |
| **比较** |                                                                                                                                                                                 |                                                                                                                                   |                                                                                                                              |                                                                                                                                                                                                                                     |
| Licenses | MIT                                                                                                                                                                             | MIT                                                                                                                               | MIT                                                                                                                          | MIT                                                                                                                                                                                                                                 |
| Created  | 5 years ago (Feb, 2011)                                                                                                                                                         | 5 years ago (Jan, 2012)                                                                                                           | 3 years ago (Jul, 2013)                                                                                                      | 4 years ago (Mar, 2012)                                                                                                                                                                                                             |
| 版本数量 | **459**                                                                                                                                                                         | 56                                                                                                                                | 63                                                                                                                           | 416                                                                                                                                                                                                                                 |
| 版本周期 | every 4 days                                                                                                                                                                    | every a month                                                                                                                     | every 18 days                                                                                                                | **every 4 days**                                                                                                                                                                                                                    |
| 依赖数   | 46                                                                                                                                                                              | 16                                                                                                                                | **13**                                                                                                                       | 15                                                                                                                                                                                                                                  |

### 什么选择了它，它解决了什么

- gulp

  配置简单，插件丰富，上手快

  解决了 grunt 配置繁琐，不易维护的问题，通过插件扩展进一步提高了开发效率

- webpack

  解决模块自由引入，打包

webpack 解决了 sf 后台存在的历史问题，提高了开发效率

1. html 模板、js 模块的自由引入，不再依赖配置表
2. 每次添加新模块，不再需要加入一堆的 script 标签
3. 代码压缩合并速度提升
4. coffee 换成了 js（es6）

Webpack 增加了项目开发的灵活性，优化了性能

1. 提取公用 js 代码
2. 只要有 loader ，可以自由使用多种语言
3. 支持按需加载

### 怎么用

#### gulp 入门  

去年在 SF 技术分享会安利过 gulp [building width gulp](https://wtser.com/building-with-gulp/)

#### 我们是怎么用 webpack 的

配置 webpack 需要建一个 webpack.config.js 文件。
建议搭配  webpack-dev-server 使用。[webpack doc webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)

我们的配置文件大概长这个样子。下面来分析一下。

```javascript
var webpack = require("webpack");

var reusePlugin = new webpack.ProvidePlugin({
  $: "jquery",
  jQuery: "jquery",
  _: "underscore",
  "window.jQuery": "jquery",
  "root.jQuery": "jquery"
});

var config = {
  context: __dirname + "/src",
  devtool: "source-map",
  entry: {
    chart: ["./chart/ChartCtrl.js", "./chart/ChartDirective.js"],
    log: "./log/ctrl.js",
    dashboard: "./dashboard/ctrl.js",
    operation: "./operation/ctrl.js"
  },
  output: {
    path: __dirname + "/dist",
    filename: "./[name]/ctrl.js",
    publicPath: "/static/dist/"
  },

  devServer: {
    hot: true,
    port: 3333,
    proxy: {
      "*": {
        target: "http://xxadmin.domain.com",
        secure: false,
        changeOrigin: true
      }
    }
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components|3rd)/,
        loader: "babel", // 'babel-loader' is also a legal name to reference
        query: {
          presets: ["es2015"]
        }
      },
      {
        test: /\.html$/,
        loader: "html"
      },
      {
        test: /\.css$/,
        exclude: /(node_modules|bower_components|3rd)/,
        loader: "style-loader!css-loader"
      },
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },
      {
        test: /\.(png|woff|woff2|eot|ttf|svg)$/,
        loader: "url-loader?limit=100000"
      }
    ]
  },
  plugins: [reusePlugin]
};

module.exports = config;
```

##### entry

处理的入口，配置需要处理的 js。entry 有三种写法，每个入口称为一个 chunk。

- **字符串  ** `entry: "./index/index.js"`

配置模块会被解析为模块，并在启动时加载。默认 chunk 名为 main， 具体打包文件名可在 output 中配置。

- **数组  ** `entry: ['./src/mod1.js', [...,] './src/index.js']`

所有的模块会在启动时  **按照配置顺序**  加载，合并到最后一个模块会被导出。默认 chunk 名也是 main。

- **对象  ** `entry: {index: '...', login : [...] }`

传入 Object，则会生成多个入口打包文件， key 是 chunk 名，value 可以是字符串，也可是数组。

很明显我们采用的是第三种。

##### output

设置入口配置的文件的输出规则，通过 output 对象实现

- output.path

输出文件路径，通常设置为 \_\_dirname + ‘/build’

- output.filename:

输出文件名称，有下面列出的四种可选的变量。

- - [id] chunk 的 id
  - [name] chunk 名
  - [hash] 编译哈希值
  - [chunkhash] chunk 的 hash 值

  filename 配置可以是这几种的任意一种或多种的组合。

- output.publicPath

设置为想要的资源访问路径。一般使用 webpack-dev-server 时，则需要通过类似 [http://localhost:8080/asstes/index-1.js](http://localhost:8080/asstes/index-1.js) 来访问资源，如果没有设置，则默认从站点根目录加载。

##### web_modules

有些时候，我们用到的第三方库并没有采用 CommonJS 或 AMD 规范。这样我们无法通过 require() 来引用这些库。

Webpack 给出了解决方案，在项目根目录下，创建一个叫做 web_modules 的文件夹，将需要用到的第三方库存放到里面，就可以在逻辑代码中使用 `require(‘xx-lib.js’)` 来引用并使用了。

##### 去除多个文件中的频繁依赖

当我们经常使用 React、jQuery 等外部第三方库的时候，通常在每个业务逻辑 JS 中都会遇到这些库。

如我们需要在各个文件中都是有 jQuery 的\$对象，因此我们需要在每个用到 jQuery 的 JS 文件的头部通过 require('jquery')来依赖 jQuery。 这样做非常繁琐且重复。

webpack 提供了我们一种比较高效的方法，我们可以通过在配置文件中配置使用到的变量名，那么 webpack 会自动分析，并且在编译时帮我们完成这些依赖的引入。

这样，我们在 JS 中，就不需要引入 jQuery 等常用模块了，直接使用配置的这些变量，webpack 就会自动引入配置的库。

```javascript
new webpack.ProvidePlugin({
  $: "jquery",
  jQuery: "jquery",
  _: "underscore",
  "window.jQuery": "jquery",
  "root.jQuery": "jquery"
});
```

##### 功能标识（Feature flags）

项目中有些代码我们只为在开发环境（例如日志）或者是内部测试环境（例如那些没有发布的新功能）中使用，又不想让这些调试内容在发布的时候泄露出去,那就需要引入下面这些魔法全局变量（magic globals）：

```
if (__DEV__) {
  console.warn('Extra logging');
}
// ...
if (__PRERELEASE__) {
  showSecretFeature();
}
```

同时还要在 webpack.config.js 中配置这些变量，使得 webpack 能够识别他们。

```
// webpack.config.js

// definePlugin 会把定义的string 变量插入到Js代码中。
var definePlugin = new webpack.DefinePlugin({
  __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV || 'true')),
  __PRERELEASE__: JSON.stringify(JSON.parse(process.env.BUILD_PRERELEASE || 'false'))
});

module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  },
  plugins: [definePlugin]
};
```

配置完成后，就可以使用  BUILD_DEV=1 BUILD_PRERELEASE=1 webpack 来打包代码了。 值得注意的是，webpack -p  会删除所有无作用代码，也就是说那些包裹在这些全局变量下的代码块都会被删除，这样就能保证这些代码不会因发布上线而泄露。

##### 异步加载

虽然 CommonJS 是同步加载的，但是 webpack 也提供了异步加载的方式。这对于单页应用中使用的客户端路由非常有用。当真正路由到了某个页面的时候，它的代码才会被加载下来。

指定你要异步加载的   拆分点。看下面的例子

```
if (window.location.pathname === '/feed') {
  showLoadingState();
  require.ensure([], function() { // 这个语法痕奇怪，但是还是可以起作用的
    hideLoadingState();
    require('./feed').show(); // 当这个函数被调用的时候，此模块是一定已经被同步加载下来了
  });
} else if (window.location.pathname === '/profile') {
  showLoadingState();
  require.ensure([], function() {
    hideLoadingState();
    require('./profile').show();
  });
}
```

剩下的事就可以交给 webpack，它会为你生成并加载这些额外的  chunk  文件。

##### 简化执行代码

我们可以在 package.json 中事先定义好命令：

```javascript
"scripts": {    
  "dev": "BUILD_DEV=1 webpack-dev-server --progress --colors",    
  "build": "BUILD_PRERELEASE=1 webpack -p"  }
```

那么就可以避免输入冗长的命令了

开发时输入 `npm run dev`

发布时输入 `npm run build`

##### 合并优化公共代码

项目中，对于一些常用的组件，站点公用模块经常需要与其他逻辑分开，然后合并到同一个文件，以便于长时间的缓存。要实现这一功能，配置参照:

```
var webpack            = require('webpack');

var CommonsChunkPlugin = webpack.optimize.CommonsChunkPlugin;
...

entry: {

   a: './index/a.js',

   b: './idnex/b.js',

   c: './index/c.js',

   d: './index/d.js'

},

...

plugins: [

   new CommonsChunkPlugin('part1.js', ['a', 'b']),

   new CommonsChunkPlugin('common.js', ['part1', 'c'])

]
```

简单的情况下可以这样写  newwebpack.optimize.CommonsChunkPlugin('common.js’);，这样就会提取所有模块的通用代码到 common.js。

##### devtool 调试

可以通过在配置中加入 devtool 项，选择预设调试工具来提高代码调试质量和效率：

- eval – 每个模块采用 eval 和  //@ sourceURL  来执行
- source-map – sourceMap 是发散的，和 output.sourceMapFilename 协调使用
- hidden-source-map – 和 source-map 类似，但是不会添加一个打包文件的尾部添加引用注释
- inline-source-map – SourceMap 以 DataUrl 的方式插入打包文件的尾部
- eval-source-map – 每个模块以 eval 方式执行并且 SourceMap 以 DataUrl 的方式添加进 eval
- cheap-source-map – 去除 column-mappings 的 SourceMap， 来自于 loader 中的内容不会被使用。
- cheap-module-source-map – 去除 column-mappings 的 SourceMap, 来自于 loader 中的 SourceMaps 被简化为单个 mapping 文件

| **devtool**                  | **构建速度** | **再次构建速度** | **支持发布版** | **质量**             |
| ---------------------------- | ------------ | ---------------- | -------------- | -------------------- |
| eval                         | +++          | +++              | no             | 生成代码             |
| cheap-eval-source-map        | +            | ++               | no             | 转换代码(lines only) |
| cheap-source-map             | +            | o                | yes            | 转换代码(lines only) |
| cheap-module-eval-source-map | o            | ++               | no             | 源代码 (lines only)  |
| cheap-module-source-map      | o            | –                | yes            | 源代码(lines only)   |
| eval-source-map              | —            | +                | no             | 源代码               |
| source-map                   | —            | —                | yes            | 源代码               |

##### loader

**来自官方文档:**
“Loaders allow you to preprocess files as you require() or “load” them. Loaders are kind of like “tasks” are in other build tools, and provide a powerful way to handle frontend build steps. Loaders can transform files from a different language like CoffeeScript to JavaScript, or inline images as data URLs. Loaders even allow you to do things like require() css files right in your JavaScript!”

```
module.exports = {

entry: ["./global.js" , "./app.js"],

output: {

filename: "bundle.js"

},
module: {

loaders: [

  {
  	test: /\.es6$/,
  	exclude: /node_modules/,
  	loader: 'babel-loader',
	query: {
  		presets: ['react', 'es2015']
  	}
  }
]

},
resolve: {
	extensions: ['', '.js', '.es6']
},
}
```

我们的第一个 loader 添加了 3 个键，下面分别做下解释。

1. **test** —  一个正则表达式，测试什么样的文件类型可以通过 loader 去执行。上面的例子意思是仅后缀为.es6 的文件通过。
2. **exclude** —  表示 loader 应该忽略／不包含的文件／文件路径。例如 node_modules 文件夹.
3. **loader** —表示我们正在使用的 loader 名称 (babel-loader).
4. **query** —  你可以传递一些选项参数到 loader，写法类似一个  [query string](https://github.com/webpack/loader-utils) 或者像上面的例子那样使用 query 属性。
5. **presets **—让我们能够使用早先安装好的 react 和 es2015 的 presets。

#### another config example

```
var webpack            = require('webpack');

var CommonsChunkPlugin = webpack.optimize.CommonsChunkPlugin;

var ExtractTextPlugin  = require('extract-text-webpack-plugin');

//自定义"魔力"变量

var definePlugin = new webpack.DefinePlugin({

    __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV || 'false')),

    __PRERELEASE__: JSON.stringify(JSON.parse(process.env.BUILD_PRERELEASE || 'false'))

});

module.exports = {

    //上下文

    context: __dirname + '/src',

    //配置入口

    entry: {

        a: './view/index/index.js',

        b: './view/index/b.js',

        vender: ['./view/index/c.js', './view/index/d.js']

    },

    //配置输出

    output: {

        path: __dirname + '/build/',

        filename: '[name].js?[hash]',

        publicPath: '/assets/',

        sourceMapFilename: '[file].map'

    },

    devtool: '#source-map',

    //模块

    module: {

        loaders: [

            {

                //处理javascript

                test: /\.js$/,

                exclude: /node_modules/,

                loader: 'babel'

            }, {

                test: /\.css$/,

                loader: ExtractTextPlugin.extract(

                    "style-loader",

                    "css-loader?sourceMap"

                )

            }, {

                test: /\.less$/,

                loader: ExtractTextPlugin.extract(

                    "style-loader",

                    "css-loader!less-loader"

                )

            }, {

                test: /\.(png|jpg)$/,

                loader: 'url-loader?limit=1024'

            }, {

                //处理vue

                test: /\.vue$/,

                loader: 'vue-loader'

            },

            {

                test: /\.woff(\?v=\d+\.\d+\.\d+)?$/,

                loader: 'url?limit=10000&minetype=application/font-woff'

            },

            {

                test: /\.woff2(\?v=\d+\.\d+\.\d+)?$/,

                loader: 'url?limit=10&minetype=application/font-woff'

            },

            {

                test: /\.ttf(\?v=\d+\.\d+\.\d+)?$/,

                loader: 'url?limit=10&minetype=application/octet-stream'

            },

            {

                test: /\.eot(\?v=\d+\.\d+\.\d+)?$/,

                loader: 'file'

            },

            {

                test: /\.svg(\?v=\d+\.\d+\.\d+)?$/,

                loader: 'url?limit=10&minetype=image/svg+xml'

            }

        ]

    },

    plugins: [

        //公用模块

        new CommonsChunkPlugin('common.js', ['a', 'b']),

        //设置抽出css文件名

        new ExtractTextPlugin("css/[name].css?[hash]-[chunkhash]-[contenthash]-[name]", {

            disable: false,

            allChunks: true

        }),

        //定义全局变量

        definePlugin,

        //设置此处，则在JS中不用类似require('./base')引入基础模块， 只要直接使用Base变量即可

        //此处通常可用做，对常用组件，库的提前设置

        new webpack.ProvidePlugin({

            Moment: 'moment', //直接从node_modules中获取

            Base: '../../base/index.js' //从文件中获取

        })

    ],

    //添加了此项，则表明从外部引入，内部不会打包合并进去

    externals: {

        jquery: 'window.jQuery',

        react: 'window.React',

        //...

    }

};
```

### 总结

工欲善其事，必先利其器。

### 参考资料

1. [webpack 常用配置总结](http://www.h-simon.com/42/)
2. [Webpack](http://segmentfault.com/a/1190000002551952) [入门指迷](http://segmentfault.com/a/1190000002551952)
3. [Webpack](http://webpack.github.io/docs/)[官方文档](http://webpack.github.io/docs/)
4. [webpack-howto](https://github.com/petehunt/webpack-howto/blob/master/README-zh.md)
5. [beginner-s-guide-to-webpack](https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.mb01dbf12)
6. [compare browserify,grunt,gulp,webpack](https://npmcompare.com/compare/browserify,grunt,gulp,webpack)
