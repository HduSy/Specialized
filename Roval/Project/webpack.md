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

### devtool

选择一种 [sourcemap](http://blog.teamtreehouse.com/introduction-source-maps) 风格来增强调试过程，`devtool` 默认值在生产环境下，也就是 `mode` 为 `production`，值是 `false`。在开发环境下也就是 `mode` 为 `development`，值是 `eval` ，不同的值会明显影响到初次构建 `build` 和重构建 `rebuild` 的速度

| 配置值 | 含义 |
| ---- | ---- |
| `false` | 不生成 `sourcemap` |
| `inline` | `base64` 内联的 `sourcemap` |
| `cheap` | 针对业务代码，定位到行 |
| `module` | 可以定位业务代码 + 第三方依赖代码 |
| `eval` | `eval` 函数包裹，打包速度最快，性能最好 |
| `hidden` | 生成但不使用 |

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

### context

一个**绝对路径**，基础目录，用于从配置中解析入口点 (`entry`) 和 加载器 (`loader`)，`context + entry = webpack entry point`

### entry 构建入口

#### entry

```ts
/**
	 * The entry point(s) of the compilation.
	 */
	entry?:
		| string
		| (() => string | EntryObject | string[] | Promise<EntryStatic>)
		| EntryObject
		| string[];
```

每个 `HTML` 页面都有一个入口起点，单页应用 (`SPA`)：一个入口起点，多页应用 (`MPA`)：多个入口起点。

```ad-info
1. 单入口：若 `entry` 是一个 `string` 或 `string[]`，就只会生成一个 `Chunk`，这时 `Chunk` 的名称是 `main`；
2. 多入口：若 `entry` 是一个 `object`，就可能会出现多个 `Chunk`，这时 `Chunk` 的名称是 `object` 键值对里 `Key` 的名称；
```

### output 构建输出

#### path

一个**绝对路径**，`webpack` 打包输出文件在磁盘上的真实存放目录，必须是 `string` 类型的

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

#### publicPath

`public` 配置了资源存放磁盘路径，但浏览器如何知道资源存放在什么位置呢？  
`publicPath` 配置了资源访问路径  
【生产环境配置】指定发布到线上静态资源的 **URL 前缀**，在 `index.html` 中的引用资源时需要拼上该前缀

| 类型     | 含义                   | 例子                        |
| -------- | ---------------------- | --------------------------- |
| 相对路径 | 相对于当前浏览的 `index.html` 地址 | 'assets/'、'../assets/'、'' |
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

#### filename

配置打包输出**入口 Chunk** 的文件名称，仅针对入口模块。当通过多个入口起点 (entry point)、代码拆分 (code splitting) 或各种插件 (plugin) 创建多个 `bundle`，就需要借助模版和变量了  

内置变量：

- `name`: `chunk name`
- `id`： `chunk` 的唯一标识，从 0 开始
- `name`： `chunk` 的文件名
- `hash`： 打包中所有的文件计算出的 `hash`
- `chunkhash`： 根据当前 `chunk` 计算出 `hash`
- `contenthash`：根据打包时 `CSS 内容` 计算出的 `hash`，对提取出的 `CSS` 文件进行命名
- `query`： 模块的 `query`，例如，文件名 `?` 后面的字符串

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

与 `filename` 不同，仅针对==按需加载（运行中产生）==`异步chunk`，即**非入口 chunk** 的输出名称

#### crossOriginLoading

`Webpack` 输出的部分代码块可能需要异步加载，而异步加载是通过 `JSONP` 方式实现的 [[../前端面经/JavaScript面经|JavaScript面经]]，`JSONP` 的原理是动态地向 `HTML` 中插入一个 标签去加载异步资源，`crossOriginLoading` 则是用于配置这个异步插入的标签的 `crossorigin` 值

#### libraryTarget 和 library

使用 `Webpack` 构建一个可以被其他模块导入使用的库时需要用到该配置  

`library`：导出库名称  
`libraryTarget`：导出库规范，`umd | umd2 | commonjs2 | commonjs | amd | this | var | assign | window | global | jsonp`

### module 模块配置

#### rules

为不同文件类型模块配置相应 [[#^6bd27c|loaders]]

```js
rules: [
	{
		test: /\.js$/i,
		use: ["babel-loader"],
		include: /src/, // 只包含
		exclude: /node_modules/, // 排除在外
		enforce: "pre", // 调整 loader 执行顺序
	}
]
```

`resource` 被引用资源；`issuer` 资源引用者

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

### resolve 模块解析

`webpack` 启动后从配置的入口模块寻找所有依赖的模块，`resolve` 配置 `webpack` 如何寻找模块所对应的文件

#### modules

配置 `webpack` 解析第三方模块时应该搜寻的目录，默认只会去 `node_modules` 下寻找。如果项目中某个文件夹下的模块经常被导入，可特殊配置：

```json
module.exports = {
  //…
  resolve: {
    modules: [path.resolve(__dirname, 'components'), 'node_modules']
  }
};
```

```js
// 优先去 src/components 中寻找
import CustomComponent from 'CustomComponent.vue'
```

#### alias

配置路径别名，简化引入路径

```js
resolve: {
	alias: {
	  "@": path.resolve(__dirname, "src/components"),
	}
}
```

```js
import CustomComponent from '@/CustomComponent.vue'
```

#### extensions

引入模块时可省略文件扩展名，这时 `webpack` 会自动带上后缀尝试访问文件是否存在，配置先后顺序。默认值 `['.js', '.json', '.wasm']`

```json
// 先.ts、再.vue、默认.js、.json
extensions: ['.ts', '.vue', '.js', '.json']
```

#### enforceExtension

模块引入时不可省略文件扩展名

#### enforceModuleExtension

`node_modules` 中的模块引入时，不可省略文件扩展名，设为 `false`，配合 `enforceExtension` 使用

#### mainFields

当引用模块时，指明使用 `package.json` 中哪个字段 `field` 指定的文件，默认是 `main`，`webpack` 会根据 `mainFields` 的配置，按照数组里的顺序依次在 `package.json` 文件里寻找，仅使用第一个匹配的：

```json
// 配置 target === "web" 或者 target === "webworker" 时 mainFields 默认值是：
  mainFields: ['browser', 'module', 'main'],

  // target 的值为其他时(包括node)，mainFields 默认值为：
  mainFields: ["module", "main"],
```

#### externals

`webpack` 将排除依赖的构建，不打包进去，将小产物体积，可通过 `cdn` 引入

#### mainFiles

解析目录时要使用的文件名，目录中没有 `package.json` 时，指明使用该目录中哪个文件，默认是 `index.js`

```json
resolve: { mainFiles: ['index'], // 可以添加其他默认使用的文件名 }
```

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

### devServer

```shell
pnpm add webpack-dev-server -D
```

```js
devServer: {
    static: path.join(__dirname, 'dist'), // 静态文件目录
    https: true,
    port: 8888, // 端口
    client: { // 浏览器
      progress: true, // 浏览器显示编译进度
      overlay: true, // 浏览器全屏显示错误
    },
    open: false, // 自动打开
    hot: true, // 热更新
    compress: true, // 静态资源 gzip 压缩
    proxy: { // 代理请求
      '/api': 'http://localhost:3000', // /api/xx -> http://localhost:3000/api/xx
      '/api2': {
        target: 'http://localhost:3000', // /api2/xx -> http://localhost:3000/xx
        pathRewrite: {
          '/api2': '',
        },
      },
    },
  },
```

#### static

静态文件目录，定位到打包构建后输出目录

#### after

在服务内部的所有其他中间件之后， 提供执行自定义中间件的功能

#### before

在服务内部的所有其他中间件之前， 提供执行自定义中间件的功能

#### publicPath

【开发环境配置】`webpack` 打包输出的文件存在于内存里，打包后的资源要想在浏览器的**对外访问路径**为：`publicPath` + 资源文件

#### allowedHosts

白名单列表，允许一些开发服务器访问

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

### clean-webpack-plugin

A webpack plugin to remove/clean your build folder(s).  
每次打包时，自动执行了 `rm -rf output.path`

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

plugins: [
    new CleanWebpackPlugin(),
  ],
```

```ad-tip
设置`output.clean`为`true`同样达成效果，因此可省略该插件的安装
```

### copy-webpack-plugin

Copies individual files or entire directories, which already exist, to the build directory.  
拷贝文件到打包输出目录中（可配置 `from`、`to`）

```js
const CopyWebpackPlugin = require('copy-webpack-plugin')

plugins: [
    new CopyWebpackPlugin({
      patterns: [{ from: './src/public', to: 'public' }],
    }),
  ],
```

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

[[NoEmitOnErrorsPlugin | webpack 中文文档 | webpack中文文档 | webpack中文网](https://www.webpackjs.com/plugins/NoEmitOnErrorsPlugin/)

## 性能优化相关（也包含插件）

### terser-webpack-plugin

This plugin uses [terser](https://github.com/terser/terser) to minify/minimize your JavaScript.  
`terser-webpack-plugin` 使用 `terser` 压缩 `js`， `webpack` 官方推荐，且仍在维护状态，设置 `parallel` 参数启用多进程并行来提高构建速度  
`uglifyjs-webpack-plugin` 年久失修  
`webpack-parallel-uglify-plugin【deprecated】` 年久失修，开启多个子线程通过 `UglifyJS` 进行压缩  

### html-webpack-plugin

简化 `html` 模板的创建，设置 `template`，`js、css` 产物自动注入 `script`、`style or link`，同时支持压缩 `html` 代码

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

以指定 `HTML` 为模板自动引入打包输出的 js、css 资源  
[html-webpack-plugin: options配置说明](https://github.com/jantimon/html-webpack-plugin#options)  
[html-webpack-plugin: minify配置压缩html代码](https://github.com/jantimon/html-webpack-plugin#minification)

### mini-css-extract-plugin

此插件为每个包含 `css` 的 `js` 文件创建一个单独的 `css` 文件，不想嵌入 `style` 而是抽离为单独 `.css` 文件以 `link` 方式引入时，可使用该插件减小 `HTML` 体积，适用于生产环境，可以帮助优化页面初始加载速度。升级 `v4` 之后平替了 `extract-text-webpack-plugin`

```js
module: {
    rules: [
      {
        test: /\.scss$/i,
        // use: ["style-loader", "css-loader", "sass-loader"],
        // 替换 style-loader
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
    ],
  },
```

### optimize-css-assets-webpack-plugin

`v4`.用于优化和压缩 `CSS` 资源的插件。它使用 `cssnano` 进行 `CSS` 的优化和压缩，可以减小 `CSS` 文件的体积，提高页面加载速度

- 不能使用 `style-loader` 来处理 `css`，而是需要使用 `mini-css-extract-plugin` 插件的 `MiniCssExtractPlugin.loader` 将 `css` 单独抽离出来才能进行压缩
- `v4` 及以下的时候是配置在 `plugins` 里面，但是在 `v5` 中需要统一配置在 `optimization` 中

### css-minimizer-webpack-plugin

`v5`.Just like [optimize-css-assets-webpack-plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin) but more accurate with source maps and assets using query string, allows caching and works in parallel mode.  

### imagemin

压缩图片

```js
rules: [{
  test: /\.(gif|png|jpe?g|svg)$/i,
  use: [
    'file-loader',
    {
      loader: 'image-webpack-loader',
      options: {
        mozjpeg: {
          progressive: true,
        },
        // optipng.enabled: false will disable optipng
        optipng: {
          enabled: false,
        },
        pngquant: {
          quality: [0.65, 0.90],
          speed: 4
        },
        gifsicle: {
          interlaced: false,
        },
        // the webp option will enable WEBP
        webp: {
          quality: 75
        }
      }
    },
  ],
}]
```

### SplitChunksPlugin

`v4` 已移除，取而代之 `optimization.splitChunks`

```js
optimization: {
    splitChunks: {
      chunks: 'async', // 值有 `all`，`async` 和 `initial`
      minSize: 20000, // 生成 chunk 的最小体积（以 bytes 为单位）。
      minRemainingSize: 0,
      minChunks: 1, // 拆分前必须共享模块的最小 chunks 数。
      maxAsyncRequests: 30, // 按需加载时的最大并行请求数。
      maxInitialRequests: 30, // 入口点的最大并行请求数。
      enforceSizeThreshold: 50000,
      cacheGroups: {
        defaultVendors: {
          test: /[\/]node_modules[\/]/,
          priority: -10,
          reuseExistingChunk: true,
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true,
        },
      },
    },
  },
```

### webpack.HotModuleReplacementPlugin

【webpack 内置】  
启用 HMR

### webpack.DefinePlugin

- 如果该值为字符串，它将被作为**代码片段**来使用
- 如果该值不是字符串，则将被转换成**字符串**（包括函数方法）
- 如果键添加 `typeof` 作为前缀，它会被定义为 `typeof` 调用
- 如果值是一个对象，则它所有的键将使用相同方法定义

```js
new webpack.DefinePlugin({
  PRODUCTION: JSON.stringify(true), // true (Boolean)
  VERSION: JSON.stringify("5fa3b9"),  // 5fa3b9
  BROWSER_SUPPORTS_HTML5: true, // true (String)
  TWO: "1+1", // 2
  "typeof window": JSON.stringify("哈哈"), // object
  "process.env.TEST": { name: JSON.stringify("test") }, // {name: 'test'}
}),
```

```js
// .eslintrc.js
module.exports = {
  globals: { "PRODUCTION": true, "VERSION": true, }
}
```

允许在 **编译时** 将源码（业务代码）中的变量替换为其他值或表达式。在区分开发模式与生产模式进行不同的操作时非常有用  
[[DefinePlugin | webpack 中文文档 | webpack中文文档 | webpack中文网](https://www.webpackjs.com/plugins/define-plugin/)

### webpack.ProvidePlugin

不需要 `import` 或 `require` 就可以在项目中到处使用配置好的变量，简单理解就是**自动导入**功能

```js
// 不必在项目中各处手动引入 import $ from 'jquery'
plugins: [
  // 自动引入 jquery
  new webpack.ProvidePlugin({
    $: "jquery",
    React: 'react'
  }),
]
```

```js
// .eslintrc.js 防止ESlint报错，进行配置
module.exports = {
  globals: { "React": true, "$": true, }
}
```

### webpack.IgnorePlugin

可在构建模块时直接删除那些需要被排除的模块，从而提升模块的构建速度，并减少产物体积

```js
// 排除moment中不必要的多国语言模块
module.export = {
  plugins: [
    new webpack.IgnorePlugin(/^./locale$/, /moment$/)
  ]
}
```

### webpack.optimize.MinChunkSizePlugin

```js
new webpack.optimize.MinChunkSizePlugin({
	minChunkSize: 10000,
})
```

Keep chunk size above the specified limit by merging chunks that are smaller than the `minChunkSize`.

### prefetch

使用：`/* webpackPrefetch: true */`

```js
// index.js
document.getElementById("btn1").onclick = async () => {
  const imp = await import(/* webpackPrefetch: true */ "./impModule.js");
  imp.default();
};
// html中插入一段
<link rel="prefetch" as="script" href="file:///Users/rayonreal/dev/me-pro/oh-my-monorepo/packages/webpack5-giant/dist/async-70f06af2-chunk.js">
```

### preload

使用：`/* webpackPreload: true */`  

区别

1. `preload chunk` 具有中等优先级，并立即下载。`prefetch chunk` 在浏览器闲置时下载
2. `preload chunk` 会在父 `chunk` 加载时，以并行方式开始加载。`prefetch chunk` 会在父 `chunk` 加载结束后开始加载
3. `preload chunk` 会在父 `chunk` 中立即请求，用于当下时刻。`prefetch chunk` 会用于未来的某个时刻
4. 浏览器支持程度不同，需要注意

### js Tree-Shaking

生产模式下自动开启。配合 `package.json` 的 `sideEffects` 进行准确的 `Tree-Shaking`[[package.json#sideEffects]]

```json
"sideEffects": [
    "./src/**/*.css"
  ],
```

### css Tree-Shaking

`purgecss-webpack-plugin` 去除没用到的 `css` 样式

### 资源压缩

`compression-webpack-plugin`

### Scope Hoisting

生产环境下自动开启。分析模块之间的依赖关系，将那些只被引用一次的模块进行合并，减少引用的次数

### speed-measure-webpack-plugin

时间分析，分析整个打包的总耗时，以及每一个 `loader` 和每一个 `plugins` 构建所耗费的时间，从而帮助我们快速定位到可以优化 `Webpack` 的配置

```shell
SMP  ⏱  
General output time took 1.17 secs

 SMP  ⏱  Plugins
HtmlWebpackPlugin took 0.051 secs
ProgressPlugin took 0.001 secs
HotModuleReplacementPlugin took 0.001 secs

 SMP  ⏱  Loaders
css-loader, and 
postcss-loader, and 
thread-loader took 0.395 secs
  module count = 1
css-loader, and 
sass-loader took 0.189 secs
  module count = 1
modules with no loaders took 0.158 secs
  module count = 8
esbuild-loader took 0.062 secs
  module count = 5
style-loader, and 
css-loader, and 
postcss-loader, and 
thread-loader took 0.051 secs
  module count = 1
html-webpack-plugin took 0.022 secs
  module count = 1
style-loader, and 
css-loader, and 
sass-loader took 0.001 secs
  module count = 1



Build completed in 1.173s


Build completed in 0.735s
```

### webpack-bundle-analyzer

构建结果分析：

- 打包出的文件中都包含了什么；
- 每个文件的尺寸在总体中的占比，哪些文件尺寸大，思考一下，为什么那么大，是否有替换方案，是否使用了它包含的所有代码；
- 模块之间的包含关系；
- 是否有重复的依赖项，是否存在一个库在多个文件中重复？ 或者捆绑包中是否具有同一库的多个版本？
- 是否有相似的依赖库， 尝试使用一种依赖库实现相似的功能。
- 每个文件的压缩后的大小。

## webpack-loaders 加载器

^6bd27c

处理不同类型文件为模块

```ad-warning
`url-loader``file-loader`，`v5`已不再推荐，替代成 [Asset Modules | webpack](https://webpack.js.org/guides/asset-modules/)
```

### Asset Modules

- `asset/resource`：将资源文件输出到指定的输出目录，作用等同于 `file—loader`；
- `asset/inline`：将资源文件内容以指定的格式进行编码（一般为 `base64`），然后以 [data URI](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FHTTP%2FBasics_of_HTTP%2FData_URLs "https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Data_URLs") 的形式嵌入到生成的 `bundle` 中，作用等同于 `url-loader`；
- `asset/source`：将资源文件的内容以字符串的形式嵌入到生成的 `bundle` 中，作用相当于 `raw-loader`；
- `asset`：作用等同于设置了 `limit` 属性的 `url-loader`，即资源文件的大小如果小于 `limit` 的值（默认值为 `8kb`），则采用 `asset/inline` 模式，否则采用 `asset/resource` 模式

```js
{
	test: /\.(png|jpe?g|gif)$/i,
	// type: 'asset', // url-loader
	// parser: {
	// 生成DataURL内联的条件
	//   dataUrlCondition: {
	//     maxSize: 4 * 1024, // 4kb，单位为 byte，默认值为 8kb
	//   },
	// },
	
	// type: 'asset/inline', // 内联
	
	// type: 'asset/resource', // url-loader
},
```

自定义模块的输出路径：

- `output.assetModuleFilename`：对所有模块生效
- `generator.filename`：对具体某个类型的模块生效  

### css-loader

处理 `css` 文件中的 `@import` 和 `url()` 语法

```js
 {
    loader: 'css-loader',
    options: {
        // Enable CSS Modules features
        modules: true,
        importLoaders: 2,
        // 0 => no loaders (default);
        // 1 => postcss-loader;
        // 2 => postcss-loader, sass-loader
    },
},
```

[css-loader和file-loader/url-loader冲突无法显示图片\_唐霜的博客](https://www.tangshuang.net/8450.html)  
[webpack入门之图片、字体、文本、数据文件处理 - 掘金](https://juejin.cn/post/7126012733018865695)

### file-loader【v5-deprecated】

**将代码里的引入路径转换为打包输出后的访问路径**。开发时 `js import`、`css url()` 使用的引入路径，`file-loader` 将其转换为打包输出后的引入路径，保证正确访问到资源

```js
{
	test: /\.(png|jpg|gif|jpeg|webp|svg)$/,
	use: {
	  loader: 'file-loader',
	  options: {
		name: '[name]-[hash:4].[ext]',
		esModule: false, // v5
		outputPath: 'images/', // Specify where the target file(s) will be placed
		publicPath: './dist/images/', // Specify a custom public path for the target file(s).
	  },
	},
	type: "javascript/auto", // v5
    exclude: /node_modules/, // 排除 node_modules 目录
},
```

### url-loader【v5-deprecated】

封装了 `file-loader`，配置 `limit` 参数，静态资源体积小于 `limit` 时将被转为 `Base64URL` 作为文件引用路径，从而减少 `HTTP` 请求次数，否则仍使用 `file-loader` 做处理

```js
{
	test: /\.(png|jpg|gif|ttf)$/i,
	use: {
	  loader: 'url-loader',
	  options: {
		limit: 4 * 1024,
		name: '[name]-[hash:4].[ext]',
		esModule: false, // v5
		outputPath: 'images/', // Specify where the target file(s) will be placed
	  },
	},
	type: "javascript/auto", // v5
},
```

### raw-loader

`allow import file as string`

```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.txt$/i,
        use: 'raw-loader',
      },
    ],
  },
};
```

```js
import txt from './file.txt';
```

### less-loader

`test: /\.(less)$/`  
处理 less 文件为模块

### sass-loader

`test: /\.(sass|scss)$/`  
处理 `sass` 文件为模块

### sass-resources-loader

将 sass/less 变量全局化，不用每个文件重复引用

### style-loader

用来将 `css-loader` 生成的样式模块通过 `<style>` 标签插入到 `html` 中去

### postcss-loader

后处理器，首先说下 `PostCSS` 的功能（类比 `babel` 对 `js` 的编译）：

- 可以自动为 `CSS` 规则**添加浏览器兼容前缀**；
- 将最新的 `CSS` 语法转换成大多数浏览器都能理解的语法；
- `css-modules` 解决全局命名冲突问题；  

`postcss-loader` 是使用 `PostCSS` 处理 `css` 的 `loader`，利用 `js` 把 `css` 处理成 `AST`，并能在过程中调用各种插件，如 `autoprefixer`（已包含在 `postcss-preset-env` 插件内）、`cssnano` 等  

`postcss.config.js` 是 `postcss` 的配置文件：

```js
module.exports = {
  // remove autoprefixer if you had it here, it's part of postcss-preset-env
  plugins: ['postcss-preset-env'],
}
```

[postcss 官方插件集 · GitHub](https://github.com/postcss/postcss/blob/main/docs/plugins.md)

### esbuild-loader

```js
const { ESBuildMinifyPlugin } = require('esbuild-loader');
```

使用 `esbuild` 的能力提升 `webpack` 构建速度

### babel-loader

[[Babel]]  
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

配置前输出：

```js
__webpack_require__.r(__webpack_exports__);
/* harmony export */ __webpack_require__.d(__webpack_exports__, {
/* harmony export */   print: function() { return /* binding */ print; }
/* harmony export */ });
const print = str => {
  console.log(str)
}

console.log('console有副作用')
```

配置后输出：

```js
__webpack_require__.r(__webpack_exports__);
/* harmony export */ __webpack_require__.d(__webpack_exports__, {
/* harmony export */   print: function() { return /* binding */ print; }
/* harmony export */ });
var print = function print(str) {
  console.log(str);
};
console.log('console有副作用');
```

### ts-loader

用样可以编译 `ts` 到 `js`，`ts-loader` vs `babel-loader` 有啥区别呢  
[webpack入门之ts处理(ts-loadr和babel-loader的选择) - 掘金](https://juejin.cn/post/7127206384797483044)  

- 编译速度上
- 代码体积上
- 类型检测上

## 本地开发

`pnpm add webpack-server -D`

## 构建流程与原理

`webpack` 核心是**静态模块打包**，能够识别不同类型文件统一转换为 `JavaScript` 模块

### 构建流程

三大阶段：

1. 初始化
2. 构建
3. 封装

# Reference

[webpack进阶之性能优化(webpack5最新版本) - 掘金](https://juejin.cn/post/7244819106342780988) 该系列质量非常不错 🌟🌟🌟  
[webpack入门之提升开发效率的几个配置(ProvidePlugin、DefinePlugin、resolve、externals) - 掘金](https://juejin.cn/post/7241424021128364087) 🌟🌟🌟

[JELLY DESIGN | 京东零售官方设计共享平台](https://jelly.jd.com/article/6107701c22a78f01a317cd05)  

[在淘宝优化了一个大型项目，分享一些干货（Webpack，SplitChunk代码实例，图文结合） - 掘金](https://juejin.cn/post/6844904183917871117#heading-5) ^22a680

[万字webpack5教学](https://mp.weixin.qq.com/s/Ap8vWQqgGpe-PdU2EZFxDA)

[【Webpack4】CSS 配置之 postcss-loader - 掘金](https://juejin.cn/post/6844904017802297352)  
[PostCSS 8 插件迁移的二三事](https://www.w3ctech.com/topic/2226)  
[https://webpack.js.org/plugins/ignore-plugin/](内置IgnorePlugin插件优化打包体积)
