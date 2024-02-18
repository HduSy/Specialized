Created Date：2023-09-23 16:47:11  
Last Modified：2023-09-23 16:47:10

# Tags

#工程化 #前端工程化

# Content

## 开始

### 开发/生产环境

#### 开发环境

`esm` + `esbuild`，不对“源码”进行打包，而是启动一个开发服务器加载当前根目录下的 `index.html` 文件，利用浏览器原生支持 `ESM` 模块化标准直接加载 `html` 文件中的 `script`，顺着依赖加载其他 `js`  

```shell
vite
```

#### 生产环境

`Rollup`，也是以根目录下 `index.html` 文件为入口，会把入口文件中加载的 `js` 打包为一个 `js`，以 `ESM` 方式引入

```shell
vite build // 执行打包
vite preview // 预览打包好的项目
```

```ad-tip
- `index.html`中引入的多个`js`都将会被打包成一个`js`文件
- `vite`内置了`js`代码压缩（相比`webpack`简直太方便了，上手成本低
```

### 为什么生产环境仍需打包

尽管原生 `ESM` 现在得到了广泛支持，但由于**嵌套导入会导致额外的网络往返**，在生产环境中发布未打包的 `ESM` 仍然效率低下（即使使用 `HTTP/2`）。为了在生产环境中获得最佳的加载性能，最好还是将代码进行 `tree-shaking`、懒加载和 `chunk` 分割（以获得更好的缓存）

#### 那为何不用 `ESBuild` 打包而是 `Rollup`

`Vite` 目前的插件 `API` 与使用 `esbuild` 作为打包器**并不兼容**。尽管 `esbuild` 速度更快，但 `Vite` 采用了 `Rollup` 灵活的插件 `API` 和基础建设，这对 `Vite` 在生态中的成功起到了重要作用

### index.html 作为 Vite 入口文件

`Vite` 项目中的 `index.html` 存放在根目录，而非 `public` 目录，Vite 解析 `<script type="module" src="…">` ，甚至内联引入 JavaScript 的 `<script type="module">` 和引用 CSS 的 `<link href>` 也能利用 Vite 特有的功能被解析，`Vite` 将 `index.html` 视作源码和模块图的一部分

`Vite` 支持多个 `.html` 作为入口的 `多页应用模式`

[index.html | Vite 官方中文文档](https://cn.vitejs.dev/guide/#index-html-and-project-root)

## 功能

`Vite` 支持 `ES6` 模块引入方式，为打包构建场景提供了增强功能。

### 快

- 本地开发时，**源码不打包**，利用浏览器原生支持 `ESM` 模块规范的特性，将这部分工作交给了浏览器；
- `esbuild` 预构建第三方依赖，`esbuild` 天生快
- 利用文件系统缓存和浏览器缓存
- 内置原生 `ESM` 的 `HMR` 热替换
- 合并模块请求
- 按需提供代码

#### Vite vs Webpack

`Webpack` 从 `entry` 入口构建 `module graph`，处理每一个用到的模块，全部打包结束后，启动开发服务器，如果项目比较大，打包耗时增长；  
`Vite` 采用 `esbuild` 进行依赖预构建，将产物缓存在文件系统中 `node_modules/.vite/`，然后就立即启动开发服务器，根据 `index.html` 入口文件中的 `js` 去按需加载（浏览器强缓存），只根据访问到的路由提供按需加载的模块，不在当前路由的模块不会加载。缓存命中时也不会再次预构建、请求

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

内置支持 `sass less stylus`，安装相应的预处理器依赖后🉑直接使用

#### 开发/生产环境

开发环境下：`JS` 文件中以 `import` 导入的 `.css` 文件内容经处理后会插入到 `index.html` 文件的 `<style>` 标签中，同时自带 `HMR` 支持，不处理 `html <link>` 标签引入的 `CSS`

```html
<link rel="stylesheet" href="./src/styles/link.css">
<style type="text/css" data-vite-dev-id="/Users/rayonreal/dev/me-pro/MeiDragon/vite-appdev-template/src/styles/index.css">.base {
  box-sizing: border-box;
  width: 111px;
  height: 111px;
  background: url(/IMG_1487.JPG) center/contain no-repeat;
}
.vite {
  font-size: 12px;
}</style>
```

生产环境下：通过 `html <link>` 标签引入和 `import` 引入的 `CSS` 打包到一个 `CSS` 文件中，输出在项目的 `dist/assets` 目录下，以 `html <link>` 方式引入

```html
<!-- link引入、js中import引入都在index-ENAsjBG7.css -->
<link rel="stylesheet" crossorigin href="/assets/index-ENAsjBG7.css">
```

`index-ENAsjBG7.css`：

```css
.link {
    color: red
}

.base {
    box-sizing: border-box;
    width: 111px;
    height: 111px;
    background: url(/IMG_1487.JPG) center/contain no-repeat
}

.vite {
    font-size: 12px
}
```

```ad-tip
和`webpack`区分`development`、`production`使用不同插件一样，生产环境使用`mini-css-extract-plugin`抽离`css`，开发环境注入到`html <style>`标签
```

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

`redbox.module.css`

```css
.red-box {
  width: 111px;
  height: 111px;
  background-color: red;
}
```

`insertRedBox.js`

```js
import redBoxStyle from '../styles/redbox.module.css'
export function insertRedBox() {
  const div = document.createElement('div')
  div.className = redBoxStyle['red-box']
  document.body.appendChild(div)
}
```

`CSS Modules` 模块化命名了，避免了选择器名冲突  
`dev` - `style`

```html
<style type="text/css" data-vite-dev-id="/Users/rayonreal/dev/me-pro/MeiDragon/vite-appdev-template/src/styles/redbox.module.css">
._red-box_y6ano_1 {
  width: 111px;
  height: 111px;
  background-color: red;
}
</style>
```

`build` - `<link rel="stylesheet" crossorigin href="/assets/index-bmce2Ixs.css">`

```css
._red-box_y6ano_1{width:111px;height:111px;background-color:red}
```

### 支持 PostCSS 配置和使用

内置了 `postcss`，剩下的工作只需安装插件、配置好就🉑

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

- [postcss-preset-env](https://github.com/csstools/postcss-preset-env)：convert modern CSS into something most browsers can understand, determining the polyfills you need based on your targeted browsers or runtime environments.功能上🉑替代 `autoprefixer`
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

### 支持 JSON

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

`Vite` 提倡 `no-bundle`，开发时按需加载，无需全部打包后再加载。模块分为**源码（业务代码）和第三方依赖代码**，`no-bundle` 针对的是源码，对于第三方依赖而言仍需 `bundle`，且利用 `esbuild` 进行打包，秒级依赖编译速度

### 原因

#### 一 模块系统兼容性

`Vite` 基于浏览器原生 `ES` 模块规范实现开发服务，而第三方依赖的模块系统可能是 `CommonJS`/`UMD` 在 `Vite` 中无法直接运行，遇到以下情况必须处理：  

情况一：`import` 引入第三方依赖，浏览器可不知道要到 `node_modules` 目录下去找第三方依赖

```js
import axios from "axios"; // ES 模块
```

情况二：第三方依赖模块规范并不是 `ESM` 规范

```js
// node.js 导出模块  
module.exports = {  
  a: 1,  
  b: 2,  
};
```

因此，`Vite` 必须先转为 `ES` 模块规范

#### 二 加载性能

有些包将它们的 `ES` 模块构建为许多单独的文件彼此导入，每个 `import` 都会触发一次新的文件请求，因此在这种 `依赖层级深`、`涉及模块数量多` 的情况下，会触发成百上千个网络请求，巨大的请求量加上 `Chrome` 对同一个域名下只能同时支持 6 个 `HTTP` 并发请求的限制，导致页面加载十分缓慢

```ad-example
`lodash-es` 有超过 300 个内置模块！当我们执行 `import { debounce } from 'lodash-es'` 时，浏览器同时发出 300 多个 HTTP 请求！即使服务器能够轻松处理它们，但大量请求会导致浏览器端的网络拥塞，使页面加载变得明显缓慢，通过将 `lodash-es` 预构建成单个模块，现在我们只需要一个 HTTP 请求！
```

![[Pasted image 20240131111715.png]]  

因此，`Vite` 将拥有许多内部模块的 `ESM` 依赖项转化为单个模块，节省 `HTTP` 请求，避免网络拥塞拖慢页面加载速度

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

##### 自动开启

预构建的依赖项存放于 `node_modules/.vite` 目录，后序启动开发服务时，如果缓存中能找到直接使用，跳过预构建步骤

![[Pasted image 20240131111951.png]]

以下任一项发生变化时引起**重新预构建**：

1. 包管理器的 `.lock` 文件；
2. `package.json` 的 `dependencies`；
3. `vite.config.js` 的 `optimizeDeps` 配置项；
4. `NODE_ENV`；
5. 补丁文件修改；

##### 手动开启

- 删除 `node_modules/.vite`  
- `optimizeDeps.force: true`  
- `vite --force`

#### 浏览器缓存

已预构建的依赖请求使用 `HTTP Header max-age=31536000, immutable` 进行**强缓存**，一旦被缓存，这些请求将永远不会再次访问开发服务器，而是直接从级存中读取，以提高开发阶段页面重载性能

![[Pasted image 20240131112324.png]]  

以下任一项发生变化时引起**重新预构建**：

1. 包管理器的.lock 文件；
2. 浏览器 `Network` 选项卡禁用缓存
3. 命令行 `--force` 选项重启
4. 重载页面

## 静态资源处理

^9c7654  
`Vite` 作为一个开箱即用的前端构建工具，默认支持 `JS`、`CSS`、`Sass`、`Less`、`JSON`、图片、`HTML` 等静态资源的处理

### public 目录

不会也不应该被 `js` 引入的资源可放在 `<root>/<public>` 目录中，打包后 `public` 目录中的资源文件将被完整复制到目标目录的**根目录下** ：  
![[Pasted image 20240131114542.png]]

```ad-tip
- 尽管在`public`目录下，🉑以绝对路径方式引入其中资源，如`<img src="/IMG_1487.JPG"/>`
- 其中的资源不应被`JS`文件引入；
- 不管里面的资源是否有被外界引用，都会直接复制到打包后的根目下；
```

### new URL(url, import.meta.url)

`import.meta.url` 代表当前模块的 `URL`，`ESM` 原生支持，与 `URL` 构造函数组合使用，通过相对路径，可以得到静态资源的完整 `URL`：

```js
const imgUrl = new URL('./logo.svg', import.meta.url).href
document.getElementById('logo-img').src = imgUrl
```

## 构建生产版本

### 公共基础路径

指定一个嵌套的公共路径下部署项目，`JS` 中引用地址，`CSS` 中的 URL 地址，`HTML` 中引用的地址都将据此地址进行替换

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

向后兼容转换 `js` 语法。This plugin provides support for legacy browsers that do not support those features when building for production.

```
npm i -D @vitejs/plugin-legacy
npm i -D terser  #必须安装Terser，因为 @vitejs/plugin-legacy插件使用Terser进行压缩JS代码
```

配合 `.browserslistrc` 作不同浏览器环境兼容

```ts
import PluginLegacy from '@vitejs/plugin-legacy'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins:[ PluginLegacy() ]
})
```

`build` 输出：

```txt
index-wgTsvOKx.js // 未兼容的，浏览器环境支持的情况下只会请求该文件
index-legacy-FcXRIZlj.js // 语法兼容处理后的
polyfills-legacy-QX10uLmV.js // API兼容
```

配合 `script nomodule` 属性，该属性表明这段脚本在支持 `esm` 浏览器的环境中不会执行

```html
<script nomodule crossorigin id="vite-legacy-polyfill" src="/assets/polyfills-legacy-QX10uLmV.js"></script>
<script nomodule crossorigin id="vite-legacy-entry" data-src="/assets/index-legacy-FcXRIZlj.js">System.import(document.getElementById('vite-legacy-entry').getAttribute('data-src'))</script>
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

## vite.config.js

```js
import PluginLegacy from '@vitejs/plugin-legacy';
import { defineConfig } from 'vite';

export default defineConfig({
  /**
   * Dep optimization options
   */
  optimizeDeps: {
    /**
     * By default, Vite will crawl your `index.html` to detect dependencies that
     * need to be pre-bundled. If `build.rollupOptions.input` is specified, Vite
     * will crawl those entry points instead.
     *
     * If neither of these fit your needs, you can specify custom entries using
     * this option - the value should be a fast-glob pattern or array of patterns
     * (https://github.com/mrmlnc/fast-glob#basic-syntax) that are relative from
     * vite project root. This will overwrite default entries inference.
     * 默认行为不符合要求时，比如入口文件为 .vue，可自定入口
     */
    entries: [],
    /**
     * Force optimize listed dependencies (must be resolvable import paths,
     * cannot be globs).
     * 指定强制预构建的依赖，Vite自身扫描检测依赖不可靠
     * 场景一：动态import会导致二次预构建=重走预构建+刷新页面+重新请求，Vite天然按需加载的特性使得某些依赖只在运行时才识别到
     * 场景二：某些包被exclude（不常用）
     */
    include: [],
    /**
     * Do not optimize these dependencies (must be resolvable import paths,
     * cannot be globs).
     */
    exclude: [],
    /**
     * Options to pass to esbuild during the dep scanning and optimization
     *
     * Certain options are omitted since changing them would not be compatible
     * with Vite's dep optimization.
     *
     * - `external` is also omitted, use Vite's `optimizeDeps.exclude` option
     * - `plugins` are merged with Vite's dep plugin
     *
     * https://esbuild.github.io/api
     * 自定 esbuild 配置
     */
    esbuildOptions: {
      plugins: []
    },
    /**
     * Force dep pre-optimization regardless of whether deps have changed.
     * 手动强制开启预构建
     * @experimental
     */
    force: true
  },
  // CSS related options (preprocessors and CSS modules)
  css: {
    // https://github.com/css-modules/postcss-modules
    modules: {
      generateScopedName: '[name]-[local]-[hash:base64:5]'
    },
    preprocessorOptions: {

    },
    postcss: {
      plugins: []
    }
  },
  // 模块解析，和`webpack`一样
  resolve: {
    alias: {
      '@': '/src/',
      '@utils': '/src/utils/',
      '@styles': '/src/styles/',
    }
  },
  // 配置开发服务
  server: {
    port: 5173,
    open: true,
    hmr: true,
    /**
     * Configure custom proxy rules for the dev server. Expects an object
     * of `{ key: options }` pairs.
     * Uses [`http-proxy`](https://github.com/http-party/node-http-proxy).
     * Full options [here](https://github.com/http-party/node-http-proxy#options).
     *
     * Example `vite.config.js`:
     * ``` js
     * module.exports = {
     *   proxy: {
     *     // string shorthand
     *     '/foo': 'http://localhost:4567/foo',
     *     // with options
     *     '/api': {
     *       target: 'http://jsonplaceholder.typicode.com',
     *       changeOrigin: true,
     *       rewrite: path => path.replace(/^\/api/, '')
     *     }
     *   }
     * }
     * ```
     */
    proxy: {
      // /foo 是字符串 http://localhost:5173/foo 的简写法
      '/foo': 'http://localhost:4567/foo',
      '/api': {
        target: 'http://jsonplaceholder.typicode.com',
        changeOrigin: true,
        rewrite: path => path.replace(/^\/api/, '')
      }
    }
  },
  // 配置打包服务
  build: {
    /**
     * Directory relative from `outDir` where the built js/css/image assets will
     * be placed.
     * @default 'assets'
     */
    assetsDir: 'assets',
    /**
     * Static asset files smaller than this number (in bytes) will be inlined as
     * base64 strings. Default limit is `4096` (4 KiB). Set to `0` to disable.
     * @default 4096
     */
    assetsInlineLimit: 5 * 1024,
    /**
     * Will be merged with internal rollup options.
     * https://rollupjs.org/configuration-options/
     */
    rollupOptions: {
      // 生产环境多入口配置，开发环境默认支持
      input: {
        index: './index.html',
        list: './list.html',
      },
      output: {
        // 入口chunk
        entryFileNames: 'assets/js/[name]-[hash:8].js',
        // 非入口chunk
        chunkFileNames: 'assets/chunk/[name]-[hash:8].js',
        // 资源出口路径(如：图片、css等)
        assetFileNames: chunkInfo => {
          const { name = '', source, type } = chunkInfo
          if (/\.css$/i.test(name)) {
            return "assets/css/[name]-[hash:8][extname]"
          } else if (/\.[jpe?g|png|gif]$/i.test(name)) {
            return "assets/images/[name]-[hash:8][extname]"
          } else {
            return `assets/[ext]/[name]-[hash:8][extname]`
          }
        }
      }
    }
  },
  // 插件
  plugins:[ PluginLegacy() ]
})
```

## 双引擎架构

![[Pasted image 20240131211828.png]]

### esbuild

负责 `依赖预构建-Bundler`、`TS(X)、JSX语法转译-Transformer`、`代码压缩-Minifier`

优点：

- `GoLang` 开发，原生机器码
- 多核多线程优势
- 无第三方依赖
- `AST` 复用，节省内存  
缺点：
- 不支持降级到 `ES5` 的代码。这意味着在低端浏览器代码会跑不起来
- 不支持 `const enum` 等语法。这意味着单独使用这些语法在 `esbuild` 中会直接抛错
- 不提供操作打包产物的接口，像 `Rollup` 中灵活处理打包产物的能力 (如 `renderChunk` 钩子) 在 `esbuild` 当中完全没有
- 不支持自定义 `Code Splitting` 策略。传统的 `Webpack` 和 `Rollup` 都提供了自定义拆包策略的 `API`，而 `esbuild` 并未提供，从而降级了拆包优化的灵活性
- 不支持 `ts` 类型检查，没有实现类型系统

### Rollup

- `vite` 插件完全兼容 `Rollup`
- `css` 代码抽离，更好利用浏览器对静态资源的缓存
- 异步加载优化，为入口 `chunk` 的依赖自动生成预加载标签 `<link rel="modulepreload">`

# Reference

[Vite | 下一代的前端工具链](https://cn.vitejs.dev/)  
[Vite | 前端那些事儿](https://jonny-wei.github.io/blog/devops/vite/engines.html)  
[[../前端面经/Vite面经|Vite面经]]
