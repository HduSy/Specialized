Created Date：2023-09-23 16:47:11  
Last Modified：2023-09-23 16:47:10

# Tags

#工程化 #前端工程化

# Content

## 开始

### index.html 作为 Vite 入口文件

`Vite` 项目中的 `index.html` 存放在根目录，而非 `public` 目录，Vite 解析 `<script type="module" src="…">` ，甚至内联引入 JavaScript 的 `<script type="module">` 和引用 CSS 的 `<link href>` 也能利用 Vite 特有的功能被解析，`Vite` 将 `index.html` 视作源码和模块图的一部分

`Vite` 支持多个 `.html` 作为入口的 `多页应用模式`

[index.html | Vite 官方中文文档](https://cn.vitejs.dev/guide/#index-html-and-project-root)

## 功能

`Vite` 支持 `ES6` 模块引入方式，为打包构建场景提供了增强功能。

### NPM 依赖解析与预构建

浏览器和原生 `ES` 不支持 [裸模块](https://juejin.cn/post/7062269629795680287#heading-0)（`bare-import`）导入，`Vite` 会检索源码中的裸模块导入，并进行以下操作：

1. 利用 `esbuild` [[#^352e73|依赖预构建]]，模块规范统一转换为 `ESM`
2. 重写替换为正确路径，如 `/node_modules/xxx/xxx/xx`

### 模块热更新

`Vite` 内置了 `HMR API` 支持热更新，无需重新加载页面或清除应用状态。内置 `HMR`:

`@vitejs/plugin-vue`: [plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue)  
`@vitejs/plugin-react`:[plugin-react](https://github.com/vitejs/vite-plugin-react/tree/main/packages/plugin-react)

### TypeScript

#### 仅支持转译，不支持类型检查

`Vite` 按需转译能够保证构建速度，而类型检查需要整个模块图，严重损害 `Vite` 速度优势。类型检查可用 [[#^6109a9|vite-plugin-checker]]

#### 编译器选项

[tsconfig.json 配置项中的注意事项](https://cn.vitejs.dev/guide/features.html#typescript-compiler-options)

### Vue

`Vite` 为 `Vue` 提供第一优先级支持：

- `Vue 3` 单文件组件支持：[@vitejs/plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue)
- `Vue 3 JSX` 支持：[@vitejs/plugin-vue-jsx](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue-jsx)
- `Vue 2.7 SFC` 支持：[@vitejs/plugin-vue2](https://github.com/vitejs/vite-plugin-vue2)
- `Vue 2.7 JSX` support via [@vitejs/plugin-vue2-jsx](https://github.com/vitejs/vite-plugin-vue2-jsx)

### 内置支持 CSS 预处理器

内置支持 `sass less stylus`，🉑直接使用

### 支持 CSS Module 配置和使用

`Vite` 原生支持 [[../dev/CSS/css#模块化|模块化]]，会对 `.module.css` 后缀结尾的文件视作 `CSS Modules`，且可自定义配置处理后类名生成规则：

```ts
export default defineConfig({
  // ...
  css: {
    modules: {
      localsConvention: 'camelCaseOnly', // 生成的样式对象类型key形式，camel or dash
      scopeBehaviour: 'local', // 是否开启css模块化
      generateScopedName: '[name]_[local]_[hash:base64:5]', // name-文件名 local-css类名
    },
  },
  // ...
})
```

### 支持 PostCSS 配置和使用

```ts
import postcssPresetEnv from 'postcss-preset-env' // 🉑编写最新CSS语法，无需担心兼容问题
import autoprefixer from 'autoprefixer' // 解决浏览器兼容问题，为CSS添加不同浏览器的兼容前缀
export default defineConfig({
  // ...
  css: {
    postcss: {
      plugins: [postcssPresetEnv(),autoprefixer({
          // 指定目标浏览器
          overrideBrowserslist: ['Chrome > 40', 'ff > 31', 'ie 11']
        })]
    }
  },
  // ...
})
```

- [postcss-preset-env](https://github.com/csstools/postcss-preset-env)：convert modern CSS into something most browsers can understand, determining the polyfills you need based on your targeted browsers or runtime environments.
- [autoprefixer](https://github.com/postcss/autoprefixer)：parse CSS and add vendor prefixes to CSS rules using values from [Can I Use](https://caniuse.com/).
- [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem)：generates rem units from pixel units. 适配移动端应用

### 支持 CSS in JS

`Vite` 作为构建侧要考虑 `选择器命名问题`、`DCE`(`Dead Code Elimination` 即无用代码删除)、`代码压缩`、`生成 SourceMap`、`服务端渲染(SSR)` 等问题，目前的两种 `CSS in JS` 方案（`styled-components`、`emotion`）均提供了对应的 `babel` 插件，在 `Vite` 中集成即可解决这些问题：

```ts
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react({
      babel: {
        plugins: [
          "babel-plugin-styled-components" // Improve the debugging experience and add server-side rendering support to styled-components
          "@emotion/babel-plugin" // Babel plugin for the minification and optimization of emotion styles.
        ]
      },
      // 注意: 对于 emotion，需要单独加上这个配置
      // 通过 `@emotion/react` 包编译 emotion 中的特殊 jsx 语法
      jsxImportSource: "@emotion/react"
    })
  ]
})
```

### 静态资源处理

[[#^9c7654|静态资源处理]]

引入一个资源，将返回解析后的 `URL`：

```js
import imgUrl from './img.png'
document.getElementById('hero-img').src = imgUrl
```

添加查询参数可改变资源引入的方式：

```js
// 显式加载资源为一个 URL
import assetAsURL from './asset.js?url'
// 以字符串形式加载资源
import assetAsString from './shader.glsl?raw'
// 加载为 Web Worker
import Worker from './worker.js?worker'
// 在构建时 Web Worker 内联为 base64 字符串
import InlineWorker from './worker.js?worker&inline'
```

### 导入 JSON

`Vite` 支持直接导入 `JSON` 文件

### Glob 方式导入

`Vite` 支持通过 `import.meta.glob` 函数实现以 [fast-glob](https://github.com/mrmlnc/fast-glob) 方式批量导入文件：

```js
const modules = import.meta.glob('./dir/*.js')
// 转译后
// vite 生成的代码
const modules = {
  './dir/foo.js': () => import('./dir/foo.js'),
  './dir/bar.js': () => import('./dir/bar.js'),
}
// 遍历访问
for (const path in modules) {
  modules[path]().then((mod) => {
    console.log(path, mod)
  })
}
```

还有很多其他用法详见 [Glob导入 | Vite 官方中文文档](https://cn.vitejs.dev/guide/features.html#glob-import)  

### 动态导入

`Vite` 导入路径支持变量：  

```js
const module = await import(`./dir/${file}.js`)
```

### 构建优化

`Vite` 内置

#### CSS 代码分割

`Vite` 将 `chunk` 中的 `css` 代码抽离为单独文件，待 `chunk` 加载完后，以 `link` 标签插入，`chunk` 会在 `css` 加载完毕后再执行，以避免 `FOUC` 首屏闪烁问题

#### 异步加载优化

`Vite` 会通过预加载消除不必要的网络往返，同时请求

## 使用插件

`Vite` 拥有优秀的插件接口设计

### 强制插件执行顺序

`enforce:'pre'|'post'|默认`

### 区分开发/生产环境按需使用

`apply: 'build'|'serve'`

## 依赖预构建

^352e73

`Vite` 加载站点前预构建了项目依赖

### 原因

#### 一 模块系统兼容性

`Vite` 开发服务器将所有代码识别为 `ES` 模块，而第三方依赖的模块系统可能是 `CMD`/`UMD`，因此 `Vite` 必须先转为 `ES` 模块规范。

#### 二 加载性能

`Vite` 将拥有许多内部模块的依赖项转化为单个模块，节省 HTTP 请求，避免网络拥塞，拖慢页面加载速度

```ad-tip
依赖预构建仅适用于开发模式
```

### 自动搜寻依赖

如果没有找到现有缓存，`Vite` 会扫描源码找到依赖，作为预构建入口点。

### Monorepo 和链接依赖

`Vite` 会自动将链接依赖视为非 `node_modules` 里的依赖，把它当作源码，并不会打包它的依赖，而是分析它的依赖列表。

### 自定义行为

`Vite` 默认依赖发现算法不理想时，如无法直接发现 `import`，可自定义 `include/exclude` 配置项。

### 缓存

#### 文件系统缓存

预构建的依赖项存放于 `node_modules/.vite` 目录，以下任一项发生变化时引起**重新预构建**：

1. 包管理器的 `.lock` 文件；
2. `vite.config.js` 相关字段；
3. `NODE_ENV`；
4. 补丁文件修改；

或命令行加 `--force` 强制重新运营预构建

#### 浏览器缓存

已预构建的依赖请求使用 HTTP Header `max-age=31536000, immutable` 进行**强缓存**，以提高开发阶段页面重载性能，以下任一项发生变化时引起**重新预构建**：

1. 包管理器的.lock 文件；
2. 浏览器 `Network` 选项卡禁用缓存
3. 命令行 `--force` 选项重启
4. 重载页面

## 静态资源处理

^9c7654

### public 目录

不会被引入；保持原文件名；的资源可放在 `<root>/<public>` 目录中，打包后目录中的资源文件将被完整复制到目标目录的根目录下

```ad-tip
要以根绝对路径方式引入其中资源；
其中的资源不应被JS文件引用；
```

### new URL(url, import.meta.url)

`import.meta.url` 代表当前模块的 `URL`，`ESM` 原生支持，与 `URL` 构造函数组合使用，通过相对路径，可以得到静态资源的完整 `URL`：

```js
const imgUrl = new URL('./logo.svg', import.meta.url).href
document.getElementById('logo-img').src = imgUrl
```

## 构建生产版本

### 公共基础路径

指定一个嵌套的公共路径下部署项目，`JS` 中饮用地址，`CSS` 中的 URL 地址，`HTML` 中引用的地址都将据此地址进行替换

### 自定义构建

调整底层 `rollup` 选项

```js
// vite.config.js
export default defineConfig({
  build: {
    rollupOptions: {
      // https://rollupjs.org/configuration-options/
    },
  },
})
```

### 文件变化时重新构建

`vite build --watch`

```js
// vite.config.js
export default defineConfig({
  build: {
    watch: {
      // https://rollupjs.org/configuration-options/#watch
    },
  },
})
```

### 开发库

将不想打包进库的第三方依赖外部化处理，如 `react`

```js
// vite.config.js
import { resolve } from 'path'
import { defineConfig } from 'vite'

export default defineConfig({
  build: {
    lib: {
      // Could also be a dictionary or array of multiple entry points
      entry: resolve(__dirname, 'lib/main.js'),
      name: 'MyLib',
      // the proper extensions will be added
      fileName: 'my-lib',
    },
    rollupOptions: {
      // 确保外部化处理那些你不想打包进库的依赖
      external: ['vue'],
      output: {
        // 在 UMD 构建模式下为这些外部化的依赖提供一个全局变量
        globals: {
          vue: 'Vue',
        },
      },
    },
  },
})

```

## 部署静态站点

## 环境变量和模式

### 环境变量

`vite` 在 `import.meta.env` 上暴露环境变量。生产环境不支持动态替换，动态 key 取值 `import.meta.env[key]` 是无效的。

### .env 文件

`Vite` 支持从环境目录中的 `dotenv` 下列文件中加载**环境变量**

``` js
.env                # 所有情况下都会加载
.env.local          # 所有情况下都会加载，但会被 git 忽略
.env.[mode]         # 只在指定模式下加载
.env.[mode].local   # 只在指定模式下加载，但会被 git 忽略
```

`.env` 类文件会在 Vite 启动一开始时被加载，而改动会在**重启服务器后生效**。

运行时，指定 `--mode mode` 参数，去加载相应的环境变量。

为防止环境变量意外泄漏，`vite` 只暴露指定前缀的环境变量，默认为 `VITE_`。可通过 `envPrefix` 自定义环境变量前缀。

### HTML 环境变量替换

### 模式

开发（dev）：`development`  
生产（build）：`production`  
[模式 | Vite 官方中文文档](https://cn.vitejs.dev/guide/env-and-mode.html#modes)

## Vite 插件

### vite-plugin-checker

^6109a9

`Vite` 打包工具，开启一个 `worker` 支持运行 `TypeScript, VLS, vue-tsc, ESLint, Stylelint` 类型与语法检查  
[TypeScript | Vite 官方中文文档](https://cn.vitejs.dev/guide/features.html#typescript)  
[vite-plugin-checker|教程](https://vite-plugin-checker.netlify.app/)

### @vitejs/plugin-legacy

^0efffb

自动生成传统版本的 `chunk` 及与其相对应 `ES` 语言特性方面的 `polyfill`

```ts
// vite.config.js
import legacy from '@vitejs/plugin-legacy'

export default {
  plugins: [
    legacy({
      targets: ['defaults', 'not IE 11'],
    }),
  ],
}
```

### vite-plugin-compression

[GitHub - vbenjs/vite-plugin-compression: Use gzip or brotli to compress resources](https://github.com/vbenjs/vite-plugin-compression)

### vite-plugin-top-level-await

`Vite` 开发时，让普通浏览器也支持模块顶层编写 `await`，而不用额外设置 `build.target` to `esnext`

[Plugins | Vite](https://vitejs.dev/plugins/)  
[vite-plugin-top-level-await - npm](https://www.npmjs.com/package/vite-plugin-top-level-await)  

## API

### 插件 API

## 配置

### 配置智能提示

```js
export declare interface ConfigEnv {
	command: 'build' | 'serve';
	mode: string; // 'development'|'production'|...
	/**
	* @experimental
	*/
	ssrBuild?: boolean;
}
```

```js
import { defineConfig } from 'vite'
export default defineConfig(config:UserConfig|UserConfigFnObject)
```

# Reference

[Vite | 下一代的前端工具链](https://cn.vitejs.dev/)

[[../前端面经/Vite面经|Vite面经]]
