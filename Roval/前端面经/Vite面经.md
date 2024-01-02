Created Date：2023-11-06 14:54:37  
Last Modified：2023-11-06 14:54:37

# Tags

#前端面试

# Content

## 如何指定插件执行顺序

[插件 API | Vite 官方中文文档](https://cn.vitejs.dev/guide/api-plugin.html#plugin-ordering)

## Vite 插件常见 Hook

[插件 API | Vite 官方中文文档](https://cn.vitejs.dev/guide/api-plugin.html#vite-specific-hooks)

### config(config, {mode, command})

修改或返回 `config`。第一个参数：用户原始配置；第二个参数：描述配置环境的变量。

### configResolved

读取并存储最终解析到的 `config`

### configServer

配置开发服务器，拿到开发服务器实例、注入 `middleware`

### transfromIndexHtml

转换 `HTML`

### handleHotUpdate

执行自定义 `HMR`

## Vite 比 Webpack 快在哪里

1. 不需要全量构建
2. `esbuild` 打包器快
3. 动态引入，按需加载，请求相应路由时才处理 `Vite load resolve transform parse`
4. 充分利用缓存，依赖强缓存，源码协商缓存，减少请求

## Vite 相比 Webpack 缺点

1. 首次启动时首屏加载
2. 热更新慢
3. 生态没有 `Webpack` 好，且仅支持 `ESM` 模块规范  

[Vite 构建原理 | 前端那些事儿](https://jonny-wei.github.io/blog/devops/vite/building.html#q3-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%AF%B4-vite-%E6%AF%94-webpack-%E8%A6%81%E5%BF%AB)

## 为什么需要预构建

1. 因为 `Vite` 是基于浏览器原生支持 `ESM` 规范实现的 `dev server`，无论业务代码还是第三方模块，符合 `ESM` 规范才能够正常运行。但实际上第三方依赖实现规范很多无法控制，需要先转换成 `ESM` 格式的产物；
2. 请求瀑布流问题：依赖层级深、涉及模块数量多的第三方模块，每 `import` 一个模块都会触发一次请求，由于 `Chrome` 对同域名下并发请求数有限制，这样大量的请求会导致页面加载十分缓慢；

### 预构建做了什么

1. 将非 `ESM` 规范（如 `UMD` 和 `CommonJS`）的模块转换为 `ESM` 模块，使其在浏览器通过 `<script type="module"><script>` 的方式正常加载；
2. 打包第三方库代码，将分散的文件合并到一起，减少 `HTTP` 请求数，避免页面加载性能劣化；  

这两件事情由 `Esbuild` 完成，而非 `Rollup`/`Webpack`，启动飞快的原因

### 预构建开启

#### 自动开启

`node_modules/.vite`：预构建产物存放目录

#### 手动开启

1. 删除 `node_modules/.vite` 目录；
2. 在 Vite 配置文件中，将 `server.force` 设为 `true`。(注意，Vite 3.0 中配置项有所更新，你需要将 `optimizeDeps.force` 设为 `true`)；
3. 命令行执行 `npx vite --force` 或者 `npx vite optimize`；

#### 预构建配置

```js
// vite.config.ts
{
  // 预构建相关的配置项
  optimizeDeps: {
    // 自定义预构建入口文件：为一个字符串数组，默认项目中所有.html文件作为入口文件，逐个扫描其依赖
    entries: ["./src/main.vue"];
	// 手动添加一些依赖：配置为一个字符串数组，将 `lodash-es` 和 `vue`两个包强制进行预构建
	include: ["lodash-es", "vue"];
	// 自定义Esbuild行为，常用的场景是加入一些 Esbuild 插件
	esbuildOptions: {
	   plugins: [
		// 加入 Esbuild 插件
	  ];
	}
  }
}
```

##### 使用场景一：动态 import

`Vite` 按需加载，运行时发现新的依赖，重新进行依赖预构建，并刷新页面。这个过程也叫**二次预构建**，成本极大，预构建的流程重新运行一遍，还得重新刷新页面，并且需要重新请求所有的模块。`include` 里将动态 `import` 的模块引入，可避免二次预构建

##### 使用场景二：某些包被手动 exclude

预构建排除，不常用

### Vite 双引擎架构

#### 性能利器——Esbuild

##### 快

1. **使用 Golang 开发**，构建逻辑代码直接被编译为原生机器码，而不用像 JS 一样先代码解析为字节码，然后转换为机器码，大大节省了程序运行时间。
2. **多核并行**。内部打包算法充分利用多核 CPU 优势，所有的步骤尽可能并行，这也是得益于 Go 当中多线程共享内存的优势。
3. **从零造轮子**。 几乎没有使用任何第三方库，所有逻辑自己编写，大到 AST 解析，小到字符串的操作，保证极致的代码性能。
4. **高效的内存利用**。Esbuild 中从头到尾尽可能地复用一份 AST 节点数据，而不用像 JS 打包工具中频繁地解析和传递 AST 数据（如 `string -> TS -> JS -> string`)，造成内存的大量浪费。

##### 依赖预构建（Bundler）——作为 Bundle 工具

`Vite2.x` 采用 `Esbuild` 来完成第三方依赖的预构建

##### 单文件编译（Transformer）——作为 TS 和 JSX 编译工具

在 `ts(x)/js(x)` 单文件编译上面，`Vite` 也使用 `Esbuild` 进行语法转译，但是 `Esbuild` 并没有实现 `TS` 的类型系统，无法进行类型检查，需借助 `ts` 的编辑器插件或 `tsc` 命令

##### 代码压缩（Minifier）——作为压缩工具

进行生产环境的代码压缩，包括 `JS` 代码和 `CSS` 代码。  

`Terser` 其实很慢，主要有 2 个原因：

1. 压缩这项工作涉及大量 `AST` 操作，并且在传统的构建流程中，`AST` 在各个工具之间==无法共享==，比如 Terser 就无法与 Babel 共享同一个 AST，造成了很多重复解析的过程；
2. JS 本身属于 `解释性+JIT`（即时编译） 的语言，对于压缩这种 CPU 密集型的工作，其性能远远比不上 Golang 这种原生语言。

#### 构建基石——Rollup

`Vite` 用作生产环境打包的核心工具，也直接决定了 `Vite` 插件机制的设计

1. **CSS 代码分割**。如果某个异步模块中引入了一些 CSS 代码，Vite 就会自动将这些 CSS 抽取出来生成单独的文件，提高线上产物的 ==缓存复用率==；
2. **自动预加载**。`Vite` 会自动为入口 `chunk` 的依赖自动生成预加载标签 `<link rel="modulepreload">`。这种适当预加载的做法会让浏览器提前下载好资源，优化页面性能；
3. 异步 `Chunk` 加载优化。在异步引入的 `Chunk` 中，通常会有一些公用的模块，如现有两个异步引入的 `Chunk: A 和 B`，而且两者有一个 `公共依赖 C` 一般情况下，`Rollup` 打包之后，会先请求 `A`，然后浏览器在加载 `A` 的过程中才决定请求和加载 `C`，但 `Vite` 进行优化之后，请求 `A` 的同时会自动预加载 `C`，通过优化 `Rollup` 产物依赖加载方式节省了不必要的网络开销。

##### 兼容插件机制

`Vite` 的插件写法完全兼容 `Rollup`

## Vite 核心原理

### 基于 `ESM` 的 `Dev Server`

启动开发服务器，`import` 时动态请求对应文件，构建速度不随应用复杂度上升而下降

### 基于 `ESM` 的 `HMR`

# Reference

[Vite 构建原理 | 前端那些事儿](https://jonny-wei.github.io/blog/devops/vite/building.html)  
[牛客网付费 - 前端面试必备 | Vite 篇（P1-30）](https://www.nowcoder.com/discuss/526005909554843648)  
[面试必备：常见的webpack / Vite面试题汇总 - 掘金](https://juejin.cn/post/7207659644487893051)  
[谈谈你对vite的了解](https://blog.51cto.com/u_14627797/6316959)
