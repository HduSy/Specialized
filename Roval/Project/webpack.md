Created Date：2022-12-17 22:27:40  
Last Modified：2022-12-17 22:27:40

# Tags

#工程化 #前端性能优化

# Content

## 基础 config

### stats

精准控制哪些打包输出信息需要显示出来  
[Stats | webpack](https://webpack.js.org/configuration/stats/)

### output

#### path

`webpack` 打包输出文件存放目录

#### publicPath

【生产环境配置】指定静态资源在 `index.html` 中的引用地址**前缀**

| 类型     | 含义 |
| -------- | ---- |
| 相对路径 | 相对于 index.html 地址    |
| 绝对路径 |   相对于根服务器地址 `/`   |
| CDN 地址         |   相对于 CDN 指定地址   |

`html-webpack-plugin` 中配置的 `publicPath` 优先级较高

#### filename

配置打包输出 bundle 的命名，仅针对初始化模块

#### chunkFilename

配置打包输出 bundle 的命名，仅针对按需加载的非初始化模块

### resolve

#### modules

配置模块查找目录

#### extensions

按顺序以文件类型解析引用时省略了后缀的模块

#### alias

配置路径别名，缩短引入路径

### module

#### strictExportPresence

【已废弃】无效导出时 error 而不是 warning,由 parser.javascript.exportsPresence 替代

```js
parser: {
	javascript: {
		exportsPresence: 'error'
	}
}
```

#### rules

为不同文件类型模块配置相应 [[#^6bd27c|loaders]]

### devServer

#### publicPath

【开发环境配置】`webpack` 打包输出的文件存在于内存里，打包后的资源要想在浏览器的**对外访问路径**为：`publicPath` + 资源文件

### performance

[Performance | webpack](https://webpack.js.org/configuration/performance/)

配置项目静态资源、入口文件达到**文件大小限制**时，以什么级别 log 提示

#### hints

打开或关闭提示

### optimization

[Optimization | webpack](https://webpack.js.org/configuration/optimization/)

#### minimizer

覆盖

```js
minimizer: [
	new ESBuildMinifyPlugin({
		target: 'es2015', // Syntax to compile to (see options below for possible values)
		css: true,
	}),
]
```

#### runtimeChunk

`'single'`：creates a runtime file to be shared for all generated chunks.  
`true` or `'multiple'`：adds an additional chunk containing only the runtime to each entrypoint.  

[Optimization | optimization.runtimeChunk](https://webpack.js.org/configuration/optimization/#optimizationruntimechunk)

#### splitChunks

进一步制定代码分割策略，减小首屏加载体积，提高首屏加载性能 [[#^22a680|掘金优质文章]]

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      //在cacheGroups外层的属性设定适用于所有缓存组，不过每个缓存组内部可以重设这些属性
      chunks: "async", //将什么类型的代码块用于分割，三选一： "initial"：入口代码块 | "all"：全部 | "async"：按需加载的代码块
      minSize: 30000, //大小超过30kb的模块才会被提取
      maxSize: 0, //只是提示，可以被违反，会尽量将chunk分的比maxSize小，当设为0代表能分则分，分不了不会强制
      minChunks: 1, //某个模块至少被多少代码块引用，才会被提取成新的chunk
      maxAsyncRequests: 5, //分割后，按需加载的代码块最多允许的并行请求数，在webpack5里默认值变为6
      maxInitialRequests: 3, //分割后，入口代码块最多允许的并行请求数，在webpack5里默认值变为4
      automaticNameDelimiter: "~", //代码块命名分割符
      name: true, //每个缓存组打包得到的代码块的名称
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/, //匹配node_modules中的模块
          priority: -10, //优先级，当模块同时命中多个缓存组的规则时，分配到优先级高的缓存组
        },
        default: {
          minChunks: 2, //覆盖外层的全局属性
          priority: -20,
          reuseExistingChunk: true, //是否复用已经从原代码块中分割出来的模块
        },
      },
    },
  },
};

```

### watch

开启 `watch` 模式，默认为 `false`，生产环境必须为 `false`.In [webpack-dev-server](https://github.com/webpack/webpack-dev-server) and [webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware) **watch mode is enabled** by default.

## plugins

打包过程做一些处理工作

### terser-webpack-plugin

压缩 `JS` 代码  
`uglifyjs-webpack-plugin` 单线程压缩  
`webpack-parallel-uglify-plugin` 开启多个子线程通过 `UglifyJS` 进行压缩  
`terser-webpack-plugin` 使用 `terser` 压缩 `js`， `webpack` 官方推荐，且仍在维护状态

### html-webpack-plugin

简化 `html` 模板的创建，同时压缩 `html` 代码

```js
new HtmlWebpackPlugin({
	inject: true,
	chunks: 'all',
	minify: {
		removeComments: true,
		collapseWhitespace: true,
		removeRedundantAttributes: true,
		useShortDoctype: true,
		removeEmptyAttributes: true,
		removeStyleLinkTypeAttributes: true,
		keepClosingSlash: true,
		minifyJS: true,
		minifyCSS: true,
		minifyURLs: true,
	},
	template: path.resolve(ROOT_DIR, 'public/index.ejs'),
	externals: {
		React: `/javascripts/react.${buildKey}.js`,
		ReactDOM: `/javascripts/react-dom.${buildKey}.js`,
	},
})
```

以指定 HTML 为模板自动引入打包输出的 js、css 资源  
[html-webpack-plugin: options配置说明](https://github.com/jantimon/html-webpack-plugin#options)  
[html-webpack-plugin: minify配置压缩html代码](https://github.com/jantimon/html-webpack-plugin#minification)

### mini-css-extract-plugin

升级 webpack4 之后替代了 `extract-text-webpack-plugin` 将 CSS 从 JavaScript 中提取出来的插件，它会创建一个单独的 CSS 文件。这个插件适用于生产环境，可以帮助优化页面加载速度

### optimize-css-assets-webpack-plugin

用于优化和压缩 CSS 资源的插件。它使用 cssnano 进行 CSS 的优化和压缩，可以减小 CSS 文件的体积，提高页面加载速度

### VueLoaderPlugin

```js
const { VueLoaderPlugin } = require('vue-loader')
```

使得其他规则 `rules` 同样应用到 `.vue` 文件的 `<style>`、`<script>` 标签，通过改 `plugin` 施展魔法，详见代码示例 [[2023-07-11#^f8b58a]]

### case-sensitive-paths-webpack-plugin

不同 OS 下严格匹配（大小写敏感）模块引入时所在磁盘路径，统一跨平台引入路径大小写。官网描述：This Webpack plugin enforces the entire path of all required modules match the exact case of the actual path on disk。 [case-sensitive-paths](https://www.npmjs.com/package/case-sensitive-paths-webpack-plugin)

### monaco-editor-webpack-plugin

Monaco-Editor 引入。[monaco-editor-webpack-plugin](https://www.npmjs.com/package/monaco-editor-webpack-plugin)

### webpack.NoEmitOnErrorsPlugin

【webpack 内置】  
遇到编译报错不输出。比如我们启用热加载开发时，改错资源引用将导致页面实时报错，配置该插件可以让遇到错误的编译不再输出资源文件，页面也不会更新报错。打包时也是如此，遇到错误将跳过输出。  

[[NoEmitOnErrorsPlugin | webpack 中文文档 | webpack中文文档 | webpack中文网](https://www.webpackjs.com/plugins/NoEmitOnErrorsPlugin/)]()

### webpack.HotModuleReplacementPlugin

【webpack 内置】  
启用 HMR

### webpack.DefinePlugin

```js
new webpack.DefinePlugin({
	'process.env.BABEL_ENV': JSON.stringify('development'),
	'process.env.NODE_ENV': JSON.stringify('development'),
	'process.env.BROWSER': true,
	'process.env.DEBUG': true,
	__DEBUG__: true,
})
```

允许在 **编译时** 将你代码中的变量替换为其他值或表达式。这在需要根据开发模式与生产模式进行不同的操作时，非常有用。  
[[DefinePlugin | webpack 中文文档 | webpack中文文档 | webpack中文网](https://www.webpackjs.com/plugins/define-plugin/)]()

### webpack.optimize.MinChunkSizePlugin

```js
new webpack.optimize.MinChunkSizePlugin({
	minChunkSize: 10000,
})
```

Keep chunk size above the specified limit by merging chunks that are smaller than the `minChunkSize`.

## loaders

^6bd27c

处理不同类型文件为模块

### file-loader

指定静态资源 **输出** 目录地址，通过 **hash 命名** 文件活得更好缓存。开发时，使用文件相对引用路径，打包输出后自动替换文件路径为正确 URL

### url-loader

`test: /\.(png|jpg|gif|ttf)$/i`  
封装了 `file-loader`，配置 `limit` 参数，静态资源体积小于 `limit` 时将被转为 Base64URL 作为文件引用路径，从而减少 HTTP 请求次数

### less-loader

`test: /\.(less)$/`  
处理 less 文件为模块

### sass-loader

`test: /\.(sass|scss)$/`  
处理 sass 文件为模块

### sass-resources-loader

将 sass/less 变量全局化，不用每个文件重复引用

### css-loader

处理 css 文件中的 `@import` 和 `url()` 语法，输出 js 模块

### style-loader

用来将 `css-loader` 生成的样式模块通过 `<style>` 标签插入到 html 中去

### postcss-loader

利用 js 处理 css 成 AST，并能在过程中调用各种插件，如 autoprefixer、cssnano 等

### esbuild-loader

```js
const { ESBuildMinifyPlugin } = require('esbuild-loader');
```

使用 `esbuild` 的能力提升 `webpack` 构建速度

# Reference

[在淘宝优化了一个大型项目，分享一些干货（Webpack，SplitChunk代码实例，图文结合） - 掘金](https://juejin.cn/post/6844904183917871117#heading-5) ^22a680

[万字webpack5教学](https://mp.weixin.qq.com/s/Ap8vWQqgGpe-PdU2EZFxDA)

[【Webpack4】CSS 配置之 postcss-loader - 掘金](https://juejin.cn/post/6844904017802297352)  
[PostCSS 8 插件迁移的二三事](https://www.w3ctech.com/topic/2226)  
[https://webpack.js.org/plugins/ignore-plugin/](内置IgnorePlugin插件优化打包体积)
