Created Date：2023-09-23 16:47:11  
Last Modified：2023-09-23 16:47:10

# Tags

#工程化 #前端工程化

# Content

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

### CSS

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

## 环境变量和模式

### 环境变量

`vite` 在 `import.meta.env` 上暴露环境变量。生产环境不支持动态替换，动态 key 取值 `import.meta.env[key]` 是无效的。

### .env

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

## Vite 插件

### vite-plugin-checker

^6109a9

`Vite` 打包工具，开启一个 `worker` 支持运行 `TypeScript, VLS, vue-tsc, ESLint, Stylelint` 类型与语法检查

# Reference

[Vite | 下一代的前端工具链](https://cn.vitejs.dev/)
