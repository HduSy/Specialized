Created Date：2022-12-17 22:27:40  
Last Modified：2022-12-17 22:27:40

# Tags

#工程化 #前端性能优化

# Content

## CLI

| Flag/Alias | Type | Description |
| ---- | ---- | ---- |
| [`--entry`](https://webpack.js.org/api/cli#entry) | `string[]` | 入口 |
| [`--config, -c`](https://webpack.js.org/api/cli#config) | `string[]` | `webpack` 配置文件 |
| [`--output-path, -o`](https://webpack.js.org/api/cli#output-path) | `string` | 输出路径，如 `./dist` |
| `--watch, -w` | `boolean` | `watch` 模式 |
| `--mode` | `string` | `development` `production` |
| [`--analyze`](https://webpack.js.org/api/cli#analyzing-bundle) | `boolean` | It invokes `webpack-bundle-analyzer` plugin to get bundle information |
| [`--extends, -e`](https://webpack.js.org/api/cli#extends) | `string[]` | 基于现有配置进行配置扩展 |
|  |  |  |

## 基础 config

### `ts` 类型声明

```ts
/**
 * Options object as provided by the user.
 */
declare interface Configuration {
	/**
	 * Set the value of `require.amd` and `define.amd`. Or disable AMD support.
	 */
	amd?: false | { [index: string]: any };
	/**
	 * Report the first error as a hard error instead of tolerating it.
	 */
	bail?: boolean;
	/**
	 * Cache generated modules and chunks to improve performance for multiple incremental builds.
	 */
	cache?: boolean | FileCacheOptions | MemoryCacheOptions;
	/**
	 * The base directory (absolute path!) for resolving the `entry` option. If `output.pathinfo` is set, the included pathinfo is shortened to this directory.
	 */
	context?: string;
	/**
	 * References to other configurations to depend on.
	 */
	dependencies?: string[];
	/**
	 * A developer tool to enhance debugging (false | eval | [inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map).
	 */
	devtool?: string | false;
	/**
	 * The entry point(s) of the compilation.
	 */
	entry?:
		| string
		| (() => string | EntryObject | string[] | Promise<EntryStatic>)
		| EntryObject
		| string[];
	/**
	 * Enables/Disables experiments (experimental features with relax SemVer compatibility).
	 */
	experiments?: Experiments;
	/**
	 * Extend configuration from another configuration (only works when using webpack-cli).
	 */
	extends?: string | string[];
	/**
	 * Specify dependencies that shouldn't be resolved by webpack, but should become dependencies of the resulting bundle. The kind of the dependency depends on `output.libraryTarget`.
	 */
	externals?:
		| string
		| RegExp
		| ExternalItem[]
		| (ExternalItemObjectKnown & ExternalItemObjectUnknown)
		| ((
				data: ExternalItemFunctionData,
				callback: (
					err?: null | Error,
					result?: string | boolean | string[] | { [index: string]: any }
				) => void
		  ) => void)
		| ((data: ExternalItemFunctionData) => Promise<ExternalItemValue>);
	/**
	 * Enable presets of externals for specific targets.
	 */
	externalsPresets?: ExternalsPresets;
	/**
	 * Specifies the default type of externals ('amd*', 'umd*', 'system' and 'jsonp' depend on output.libraryTarget set to the same value).
	 */
	externalsType?:
		| "import"
		| "var"
		| "module"
		| "assign"
		| "this"
		| "window"
		| "self"
		| "global"
		| "commonjs"
		| "commonjs2"
		| "commonjs-module"
		| "commonjs-static"
		| "amd"
		| "amd-require"
		| "umd"
		| "umd2"
		| "jsonp"
		| "system"
		| "promise"
		| "script"
		| "node-commonjs";
	/**
	 * Ignore specific warnings.
	 */
	ignoreWarnings?: (
		| RegExp
		| {
				/**
				 * A RegExp to select the origin file for the warning.
				 */
				file?: RegExp;
				/**
				 * A RegExp to select the warning message.
				 */
				message?: RegExp;
				/**
				 * A RegExp to select the origin module for the warning.
				 */
				module?: RegExp;
		  }
		| ((warning: WebpackError, compilation: Compilation) => boolean)
	)[];
	/**
	 * Options for infrastructure level logging.
	 */
	infrastructureLogging?: InfrastructureLogging;
	/**
	 * Custom values available in the loader context.
	 */
	loader?: Loader;
	/**
	 * Enable production optimizations or development hints.
	 */
	mode?: "none" | "development" | "production";
	/**
	 * Options affecting the normal modules (`NormalModuleFactory`).
	 */
	module?: ModuleOptions;
	/**
	 * Name of the configuration. Used when loading multiple configurations.
	 */
	name?: string;
	/**
	 * Include polyfills or mocks for various node stuff.
	 */
	node?: false | NodeOptions;
	/**
	 * Enables/Disables integrated optimizations.
	 */
	optimization?: Optimization;
	/**
	 * Options affecting the output of the compilation. `output` options tell webpack how to write the compiled files to disk.
	 */
	output?: Output;
	/**
	 * The number of parallel processed modules in the compilation.
	 */
	parallelism?: number;
	/**
	 * Configuration for web performance recommendations.
	 */
	performance?: false | PerformanceOptions;
	/**
	 * Add additional plugins to the compiler.
	 */
	plugins?: (
		| undefined
		| null
		| false
		| ""
		| 0
		| ((this: Compiler, compiler: Compiler) => void)
		| WebpackPluginInstance
	)[];
	/**
	 * Capture timing information for each module.
	 */
	profile?: boolean;
	/**
	 * Store compiler state to a json file.
	 */
	recordsInputPath?: string | false;
	/**
	 * Load compiler state from a json file.
	 */
	recordsOutputPath?: string | false;
	/**
	 * Store/Load compiler state from/to a json file. This will result in persistent ids of modules and chunks. An absolute path is expected. `recordsPath` is used for `recordsInputPath` and `recordsOutputPath` if they left undefined.
	 */
	recordsPath?: string | false;
	/**
	 * Options for the resolver.
	 */
	resolve?: ResolveOptionsWebpackOptions;
	/**
	 * Options for the resolver when resolving loaders.
	 */
	resolveLoader?: ResolveOptionsWebpackOptions;
	/**
	 * Options affecting how file system snapshots are created and validated.
	 */
	snapshot?: SnapshotOptionsWebpackOptions;
	/**
	 * Stats options object or preset name.
	 */
	stats?:
		| boolean
		| StatsOptions
		| "none"
		| "verbose"
		| "summary"
		| "errors-only"
		| "errors-warnings"
		| "minimal"
		| "normal"
		| "detailed";
	/**
	 * Environment to build for. An array of environments to build for all of them when possible.
	 */
	target?: string | false | string[];
	/**
	 * Enter watch mode, which rebuilds on file change.
	 */
	watch?: boolean;
	/**
	 * Options for the watcher.
	 */
	watchOptions?: WatchOptions;
}
```

### mode

告知 `webpack` 使用相应模式的内置优化，`none`, `development` 或 `production`（default）  

- `none`： 不使用任何默认优化选项  
- `development`： 为开发环境构建，Enables useful names for modules and chunks.  
- `production`：为生产环境构建，压缩、去注释，`FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin` and `TerserPlugin`

### target 编译目标

为多种环境或 `target` 构建编译代码输出，默认值为 `browserslist` 环境若没有找到相应配置，则为 `web`，还可指定其他值：  

| `target` 值       | 描述                                                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| browserslist      | **（如果 browserslist 可用，其值则为默认）** 从 `browserslist-config` 中推断出平台和 `ES` 特性
| web               | 编译为类浏览器环境里可用 （默认）                                                              |
| node              | 编译为类 Node.js 环境可用（使用 Node.js `require` 加载 chunks）  |
| webworker         | 编译成一个 WebWorker                                                                           |
| electron-main     | 编译为 [Electron](https://electronjs.org/) 主进程。                                            |
| electron-renderer | 编译为 [Electron](https://electronjs.org/) 渲染进程，使用 `JsonpTemplatePlugin`                |

### entry 构建入口

#### context

基础目录，**绝对路径**，用于从配置中解析入口点 (entry point) 和 加载器 (loader)

#### entry

每个 HTML 页面都有一个入口起点，单页应用 (`SPA`)：一个入口起点，多页应用 (`MPA`)：多个入口起点。

```ad-info
1. 若 entry 是一个 string 或 array，就只会生成一个 Chunk，这时 Chunk 的名称是 main；
2. 若 entry 是一个 object，就可能会出现多个 Chunk，这时 Chunk 的名称是 object 键值对里 Key 的名称；
```

### output 构建输出

#### path

`webpack` 打包输出文件存放目录，必须是 `string` 类型的**绝对路径**

```js
module.exports = {
  //...
  output: {
    path: path.resolve(__dirname, 'dist/assets')
    // 常见
    path: config.build.assetsRoot 
    // 其中config.build.assetsRoot = path.resolve(__dirname, '..dist') 当前工作目录下dist文件
  }
}
```

#### filename

配置打包输出**入口 Chunk** 的文件名称，仅针对入口模块。当通过多个入口起点 (entry point)、代码拆分 (code splitting) 或各种插件 (plugin) 创建多个 `bundle`，就需要借助模版和变量了  

内置变量：

- name: Chunk name
- id： Chunk 的唯一标识，从 0 开始
- name： Chunk 的名称
- hash： Chunk 的唯一标识的 Hash 值
- chunkhash： Chunk 内容的 Hash 值
- query： 模块的 query，例如，文件名 ? 后面的字符串

```js
module.exports = {
  //...
  output: {
    filename: '[name].bundle.js'
    // 或者
    filename: '[name].[hash].bundle.js'
  }
};
```

#### chunkFilename

与 `filename` 不同，仅针对==按需加载（运行中产生）==的 Chunk 输出时的文件名称，即**非入口 Chunk** 的输出名称

#### publicPath

【生产环境配置】指定发布到线上静态资源的 **URL 前缀**，在 `index.html` 中的引用资源时需要拼上该前缀

| 类型     | 含义                   | 例子                        |
| -------- | ---------------------- | --------------------------- |
| 相对路径 | 相对于 index.html 地址 | 'assets/'、'../assets/'、'' |
| 绝对路径 | 相对于根服务器地址 `/` | '/assets'                   |
| CDN 地址 | CDN 地址               | 'https://cdn.example.com/assets/'、'//cdn.example.com/assets/'                            |

```js
module.exports = {
  //...
  output: {
    // One of the below
    publicPath: 'https://cdn.example.com/assets/', // CDN（总是 HTTPS 协议）
    publicPath: '//cdn.example.com/assets/', // CDN（协议相同）
    publicPath: '/assets/', // 相对于服务(server-relative)
    publicPath: 'assets/', // 相对于 HTML 页面
    publicPath: '../assets/', // 相对于 HTML 页面
    publicPath: '', // 相对于 HTML 页面（目录相同）
  }
};
```

`html-webpack-plugin` 中配置的 `publicPath` 优先级较高

#### crossOriginLoading

`Webpack` 输出的部分代码块可能需要异步加载，而异步加载是通过 `JSONP` 方式实现的 [[../前端面经/JavaScript面经|JavaScript面经]]，`JSONP` 的原理是动态地向 `HTML` 中插入一个 标签去加载异步资源，`crossOriginLoading` 则是用于配置这个异步插入的标签的 `crossorigin` 值

#### libraryTarget 和 library

使用 `Webpack` 构建一个可以被其他模块导入使用的库时需要用到该配置  

`library`：导出库名称  
`libraryTarget`：导出库规范，`umd | umd2 | commonjs2 | commonjs | amd | this | var | assign | window | global | jsonp`

### module 模块配置

#### noParse

属性值类型： `RegExp`、`[RegExp]`、`function`，忽略==未采用模块化标准==文件的递归解析和处理，提高构建性能

```js
// 使用正则表达式  
noParse: /jquery|chartjs/

// 使用函数，从 Webpack 3.0.0 开始支持  
noParse: (content)=> {  
  // content 代表一个模块的文件路径  
  // 返回 true or false  
  return /jquery|chartjs/.test(content);  
}
```

#### parser

解析器 (parser) 细粒度配置，不同模块语法解析规则

```js
module.exports = {
  //…
  module: {
    rules: [
      {
        //…
        parser: {
          amd: false, // 禁用 AMD
          commonjs: false, // 禁用 CommonJS
          system: false, // 禁用 SystemJS
          harmony: false, // 禁用 ES2015 Harmony import/export
          requireInclude: false, // 禁用 require.include
          requireEnsure: false, // 禁用 require.ensure
          requireContext: false, // 禁用 require.context
          browserify: false, // 禁用特殊处理的 browserify bundle
          requireJs: false, // 禁用 requirejs.*
          node: false, // 禁用 __dirname, __filename, module, require.extensions, require.main 等。
          node: {…} // 在模块级别(module level)上重新配置 node 层(layer)
        }
      }
    ]
  }
}
```

#### loader

将除 `js` 以外其他文件类型转化为 `webpack` 打包模块

- 条件匹配：通过 `test`、`include`、`exclude` 命中目标文件；
- `use` 配置项为 `loader name array`，从后向前应用一组 `loader`，`enforce` 选项可强行将 `loader` 挪到第一位或最后一位；
- `loader` 支持以 `query` 的形式传入参数，开启对应功能；

```js
module: {
  rules: [
    {
      // 命中 JavaScript 文件
      test: /\.js$/,
      // 用 babel-loader 转换 JavaScript 文件
      // ?cacheDirectory 表示传给 babel-loader 的参数，用于缓存 babel 编译结果加快重新编译速度
      use: ['babel-loader?cacheDirectory'],
      // 只命中src目录里的js文件，加快 Webpack 搜索速度
      include: path.resolve(__dirname, 'src')
    },
    {
      // 命中 SCSS 文件
      test: /\.scss$/,
      // 使用一组 Loader 去处理 SCSS 文件。
      /* 处理顺序为从后到前，即先交给 sass-loader 处理，
      再把结果交给 css-loader 最后再给 style-loader。*/
      use: ['style-loader', 'css-loader', 'sass-loader'],
      // 排除 node_modules 目录下的文件
      exclude: path.resolve(__dirname, 'node_modules'),
    },
    {
      // 对非文本文件采用 url-loader 加载
      test: /\.(gif|png|jpe?g|eot|woff|ttf|svg|pdf)$/,
      use: ['url-loader'],
    },
    // 给 Loader 传入属性的方式除了有 querystring 外，还可以通过 Object 传入 也可以 
    {
      // 命中 JavaScript 文件
      test:[
        /\.jsx?$/,
        /\.tsx?$/
      ],
      // 用 babel-loader 转换 JavaScript 文件
      // ?cacheDirectory 表示传给 babel-loader 的参数，用于缓存 babel 编译结果加快重新编译速度
      use: [
        {
          loader:'babel-loader',
           options:{
             cacheDirectory:true,
           },
          // enforce:'post' 的含义是把该 Loader 的执行顺序放到最后
          // enforce 的值还可以是 pre，代表把 Loader 的执行顺序放到最前面
          enforce:'post'
        },
        // 省略其它 Loader
      ]
      include:[
        path.resolve(__dirname, 'src'),
        path.resolve(__dirname, 'tests'),
      ],
      exclude:[
        path.resolve(__dirname, 'node_modules'),
        path.resolve(__dirname, 'bower_modules'),
      ]
    },
  ]
}
```

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

### resolve 模块解析

`webpack` 启动后从配置的入口模块寻找所有依赖的模块，`resolve` 配置 `webpack` 如何寻找模块所对应的文件

#### alias

配置路径别名，简化引入路径

#### mainFields

引用模块时，指明使用 `package.json` 中哪个**字段 field**指定的文件，默认是 `main`，`webpack` 会根据 `mainFields` 的配置，按照数组里的顺序在 `package.json` 文件里寻找，仅使用第一个匹配的：

```json
// 配置 target === "web" 或者 target === "webworker" 时 mainFields 默认值是：
  mainFields: ['browser', 'module', 'main'],

  // target 的值为其他时，mainFields 默认值为：
  mainFields: ["module", "main"],
```

#### mainFiles

解析目录时要使用的文件名，目录中没有 `package.json` 时，指明使用该目录中哪个文件，默认是 `index.js`

```json
resolve: { mainFiles: ['index'], // 可以添加其他默认使用的文件名 }
```

#### modules

配置 `webpack` 解析模块时应该搜索的目录

```json
module.exports = {
  //...
  resolve: {
    modules: [path.resolve(__dirname, 'src'), 'node_modules']
  }
};
```

#### extensions

引入模块时可省略文件扩展名，这时 `webpack` 会自动带上后缀尝试访问文件是否存在，配置先后顺序

```json
// 先.ts、再.vue、默认.js、.json
extensions: ['.ts', '.vue', '.js', '.json']
```

#### enforceExtension

模块引入时不可省略文件扩展名

#### enforceModuleExtension

`node_modules` 中的模块引入时，不可省略文件扩展名，设为 `false`，配合 `enforceExtension` 使用

### optimization 构建优化

从 webpack 4 开始，会根据你选择的 mode 来执行不同的优化，不过所有的优化还是可以手动配置和重写。 [Optimization | webpack](https://webpack.js.org/configuration/optimization/)

#### minimize

Tell webpack to minimize the bundle using the [TerserPlugin](https://webpack.js.org/plugins/terser-webpack-plugin/) or the plugin(s) specified in [`optimization.minimizer`](https://webpack.js.org/configuration/optimization/#optimizationminimizer)

#### minimizer

允许提供一个或多个定制过的 `TerserPlugin` 实例，覆盖默认压缩工具 (`minimizer`)  

```js
// 插件形式
minimizer: [
	new ESBuildMinifyPlugin({
		target: 'es2015', // Syntax to compile to (see options below for possible values)
		css: true,
	}),
	new TerserPlugin({
		cache: true,
		parallel: true,
		sourceMap: true, // 在生产环境如果使用source-map则必须设为true。Must be set to true if using source-maps in production
		terserOptions: {
		  // https://github.com/webpack-contrib/terser-webpack-plugin#terseroptions
		}
  }),
]
// 函数形式
minimizer: [
      (compiler) => {
        const TerserPlugin = require('terser-webpack-plugin')
        new TerserPlugin({ /* your config 配置项如上*/ }).apply(compiler)
      ],
```

#### splitChunks

进一步制定代码分割策略，减小首屏加载体积，提高==首屏加载性能== [[#^22a680|见掘金]]

```js
module.exports = {
  //…
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

#### runtimeChunk

运行时 chunk 如何分配：

1. `'single'`：会创建一个在所有生成 `chunk` 之间共享的运行时文件  
2. `true` or `'multiple'`：为每个入口添加一个只含有 `runtime` 的额外 `chunk`

[Optimization | optimization.runtimeChunk](https://webpack.js.org/configuration/optimization/#optimizationruntimechunk)

### devServer 开发服务器

#### after

在服务内部的所有其他中间件之后， 提供执行自定义中间件的功能

#### before

在服务内部的所有其他中间件之前， 提供执行自定义中间件的功能

#### publicPath

【开发环境配置】`webpack` 打包输出的文件存在于内存里，打包后的资源要想在浏览器的**对外访问路径**为：`publicPath` + 资源文件

#### allowedHosts

白名单列表，允许一些开发服务器访问

#### compress

是否启用 gzip 压缩

#### headers

所有响应中添加自定义 header

```json
devServer: {
    headers: {
      'X-Custom-Foo': 'bar'
    }
  }
```

#### historyApiFallback

`history` 路由开发时，服务器要求任意路由命中时返回对应的 `html` 文件，`404` 时定向跳转到对应页面

#### https

默认 `http` 服务，配置为 `https` 服务

#### hot

开启 HMR

#### proxy

请求代理

### performance

[Performance | webpack](https://webpack.js.org/configuration/performance/)

性能检测提示，配置项目静态资源、入口文件达到**文件大小限制**时，以什么级别 log 提示

#### hints

打开或关闭提示

```json
 // 输出文件性能检查配置  
  performance: {  
	hints: 'warning', // 有性能问题时输出警告  
	hints: 'error', // 有性能问题时输出错误  
	hints: false, // 关闭性能检查  
	maxAssetSize: 200000, // 最大文件大小 (单位 bytes)  
	maxEntrypointSize: 400000, // 最大入口文件大小 (单位 bytes)  
	assetFilter: function(assetFilename) { // 过滤要检查的文件  
	  return assetFilename.endsWith('.css') || assetFilename.endsWith('.js');  
	}  
  }
```

### stats

精准控制哪些打包输出信息需要显示出来  
[Stats | webpack](https://webpack.js.org/configuration/stats/)

```json
stats: { // 控制台输出日志控制
    assets: true,
    colors: true,
    errors: true,
    errorDetails: true,
    hash: true,
  },
```

### watch

开启 `watch` 模式，默认为 `false`，生产环境必须为 `false`.In [webpack-dev-server](https://github.com/webpack/webpack-dev-server) and [webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware) **watch mode is enabled** by default.

## webpack-plugins 插件

打包过程做一些处理工作 [插件生态](https://webpack.js.org/awesome-webpack)

### terser-webpack-plugin

This plugin uses [terser](https://github.com/terser/terser) to minify/minimize your JavaScript.  
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
	template: path.resolve(ROOT_DIR, 'public/index.ejs'), // ejs模板
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

## webpack-loaders 加载器

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
处理 `sass` 文件为模块

### sass-resources-loader

将 sass/less 变量全局化，不用每个文件重复引用

### css-loader

处理 `css` 文件中的 `@import` 和 `url()` 语法，输出 js 模块

### style-loader

用来将 `css-loader` 生成的样式模块通过 `<style>` 标签插入到 html 中去

### postcss-loader

利用 js 处理 css 成 AST，并能在过程中调用各种插件，如 autoprefixer、cssnano 等

### esbuild-loader

```js
const { ESBuildMinifyPlugin } = require('esbuild-loader');
```

使用 `esbuild` 的能力提升 `webpack` 构建速度

### babel-loader

[babel-loader | webpack](https://webpack.js.org/loaders/babel-loader/)  
This package allows transpiling JavaScript files using [Babel](https://github.com/babel/babel) and [webpack](https://github.com/webpack/webpack). ^a0a331

```bash
npm install -D babel-loader @babel/core @babel/preset-env webpack
```

`webpack` 配置：

```js
module: {
  rules: [
    {
      test: /\.(?:js|mjs|cjs)$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: [
            ['@babel/preset-env', { targets: "defaults" }]
          ]
        }
      }
    }
  ]
}
```

## 构建流程与原理

`webpack` 核心是**静态模块打包**，能够识别不同类型文件统一转换为 `JavaScript` 模块

### 构建流程

三大阶段：

1. 初始化
   
2. 构建
3. 封装

# Reference

[在淘宝优化了一个大型项目，分享一些干货（Webpack，SplitChunk代码实例，图文结合） - 掘金](https://juejin.cn/post/6844904183917871117#heading-5) ^22a680

[万字webpack5教学](https://mp.weixin.qq.com/s/Ap8vWQqgGpe-PdU2EZFxDA)

[【Webpack4】CSS 配置之 postcss-loader - 掘金](https://juejin.cn/post/6844904017802297352)  
[PostCSS 8 插件迁移的二三事](https://www.w3ctech.com/topic/2226)  
[https://webpack.js.org/plugins/ignore-plugin/](内置IgnorePlugin插件优化打包体积)
