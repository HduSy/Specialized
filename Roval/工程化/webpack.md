Created Date：2022-12-17 22:27:40  
Last Modified：2022-12-17 22:27:40

# Tags

#工程化

# Content

## config

### output.path

`webpack` 打包输出文件存放目录

### output.publicPath

【生产环境配置】指定静态资源在 `index.html` 中的引用地址**前缀**

| 类型     | 含义 |
| -------- | ---- |
| 相对路径 | 相对于 index.html 地址    |
| 绝对路径 |   相对于根服务器地址 `/`   |
| CDN 地址         |   相对于 CDN 指定地址   |

`html-webpack-plugin` 中配置的 `publicPath` 优先级较高

### devServer.publicPath

【开发环境配置】`webpack` 打包输出的文件存在于内存里，打包后的资源要想在浏览器的**对外访问路径**为：`publicPath` + 资源文件

## plugins

打包过程做一些处理工作

### html-webpack-plugin

```js
new HtmlWebpackPlugin({
	inject: true,
	template: path.resolve(ROOT_DIR, 'public/index.ejs'),
})
```

以指定 HTML 为模板自动引入打包输出的 js、css 资源

### mini-css-extract-plugin

升级 webpack4 之后替代了 `extract-text-webpack-plugin`  
将 CSS 从 JavaScript 中提取出来的插件，它会创建一个单独的 CSS 文件。这个插件适用于生产环境，可以帮助优化页面加载速度

### optimize-css-assets-webpack-plugin

用于优化和压缩 CSS 资源的插件。它使用 cssnano 进行 CSS 的优化和压缩，可以减小 CSS 文件的体积，提高页面加载速度

### VueLoaderPlugin

```js
const { VueLoaderPlugin } = require('vue-loader')
```

使得其他规则 `rules` 同样应用到 `.vue` 文件的 `<style>`、`<script>` 标签，通过改 `plugin` 施展魔法，详见代码示例 [[2023-07-11#^f8b58a]]

### case-sensitive-paths

不同 OS 下严格匹配（大小写敏感）模块引入时所在磁盘路径。官网描述：This Webpack plugin enforces the entire path of all required modules match the exact case of the actual path on disk。 [case-sensitive-paths](https://www.npmjs.com/package/case-sensitive-paths-webpack-plugin)

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

## loaders

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

使用 esbuild 的能力提升构建速度

# Reference

[万字webpack5教学](https://mp.weixin.qq.com/s/Ap8vWQqgGpe-PdU2EZFxDA)

[【Webpack4】CSS 配置之 postcss-loader - 掘金](https://juejin.cn/post/6844904017802297352)  
[PostCSS 8 插件迁移的二三事](https://www.w3ctech.com/topic/2226)  
[https://webpack.js.org/plugins/ignore-plugin/](内置IgnorePlugin插件优化打包体积)
