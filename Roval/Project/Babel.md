Created Date：2023-07-18 14:36:02  
Last Modified：2023-07-18 14:36:01

# Tags

#babel #前端工程化 #工程化

# Content

## 功能

1. **仅**语法 (`syntax`) 转换，新语法转换为向后兼容的语法，兼容旧浏览器（仅 `JavaScript syntax` 转换，不转换 `API` 如 `Promise`、`Proxy`、`Map`、`Set`、`Symbol`、`Iterator` 和新增实例方法如 `Object.assign`、`Array.find`）；
2. 为目标环境添加垫片 `polyfill` 提供低版本浏览器不支持的功能（如全局的 `ES6` 对象以及通过 `修改原型链 prototype` 等实现）；
3. Source code transformations (codemods)

## 使用方式

### 配置入口

`.babelrc，.babelrc.js，babel.config.js、package.json`

### 命令行使用

须安装 `@babel/cli`

```shell
pnpm babel ./src/index.js -o ./dist/output.js
```

### 构建工具里配置

`webpack`：`babel-loader` [[webpack#^a0a331]]  
`rollup`：`@rollup/plugin-babel` [[Rollup#^f9ddaa]]  
`vite`：`@vitejs/plugin-legacy` [[vite#^0efffb]]  

无论使用哪种方式，`@babel/core`、`@babel/cli`、`@babel/preset-env` 及其相应配置文件都是必须的

## 使用指南

### plugin

由 `Javascript` 编写，指导 `Babel` 如何转换代码 [官网插件列表 · Babel](https://babeljs.io/docs/plugins-list)

#### @babel/plugin-transform-runtime

##### 三大作用

A plugin that enables the **re-use** of Babel's injected helper functions code to **save on codesize**.  
移除 `helpers functions`（辅助函数），将其替换为 `@babel/runtime/helpers` 中的函数引用进行复用，节省生成代码空间.

- **减少重复引用，节省代码体积**：自动移除重复的 `helpers` 引入，替换为统一引入 `@babel/runtime/helpers` 里的 `helpers`；
- **避免污染全局环境（针对库开发者）**：create `a sandboxed environment` for your code.作为 `APP` 或 `CLI Tool` 没什么问题，但作为 `lib` 提供给其他人使用时，无法确认代码运行的环境，或许会 **pollute the global scope** ；
- `import _regeneratorRuntime from '@babel/runtime/regenerator'` 支持 `Generator/async` 函数，替代全局引入的 `regenerator-runtime/runtime`；
- 自动引入 `@babel/runtime-corejs3/core-js-stable/` 替代全局引入的 `core-js/stable`

##### why

```ad-tip
Sometimes `Babel` may inject some code in the output that is the same **across files**, every file that contains some pollyfill would have **the helper function** repeated each time. and thus can be potentially **re-used**.
```

`webpack` 基于文件模块，做不到抽离函数出来复用，因为每个函数内存地址都不一样

##### install

```shell
pnpm add --save-dev @babel/plugin-transform-runtime // for 开发环境
pnpm add @babel/runtime // for 生产环境
```

##### config

相关配置项是==必须的==否则该插件不生效

```json
{
  "presets": [["@babel/env", {
    "useBuiltIns": "usage", // "entry", false
    "corejs": 3, // 2
    "modules": false // auto(default) amd umd commonjs false
  }]], // 预设合集，语法转换
  "plugins": [ ["@babel/plugin-transform-runtime", {
    "corejs": 3 // 2 配置项 required
  }] ]
}
```

###### corejs 配置项

`"corejs: false"`：开不开启，绝定对不对 `API` 进行转换，如：

```js
import "core-js/modules/es.promise.js"; // window.Promise被污染
```

开启后

```js
import _Promise from "@babel/runtime-corejs3/core-js-stable/promise"; //chorejs@3也支持 没被污染
import _Promise from "@babel/runtime-corejs2/core-js/promise"; // chorejs@2也支持 没被污染
// API 层面
var promiseFn = function promiseFn() {
  return new _Promise(function (rs, rj) {
    try {
      setTimeout(function () {
        rs(true);
      }, 1000);
    } catch (error) {
      rj(error);
    }
  });
};
// ...
```

`"corejs: 3"` 或 `"corejs: 2"`，决定用 `corejs@2` 还是 `corejs@3` 做 `API` 转换，而 `"corejs: 3"` 能够对 `corejs@2` 不支持的 `API` 转换，如：

```js
// 配置 3
import _findInstanceProperty from "@babel/runtime-corejs3/core-js-stable/instance/find";
import _flatInstanceProperty from "@babel/runtime-corejs3/core-js-stable/instance/flat";
// 配置 2 chorejs@2 转换不了的仍有污染，转换得了的无污染
import _Promise from "@babel/runtime-corejs2/core-js/promise";
import "core-js/modules/es.array.flat.js";
import "core-js/modules/es.array.unscopables.flat.js";
```

### presets

预设，不用一个一个地去设置 `plugin`，而是同时设置一组**集合**

#### @babel/preset-flow

#### @babel/preset-react

#### @babel/preset-typescript

#### @babel/preset-env

`.browserslistrc` 配置支持的目标环境（浏览器版本、浏览器覆盖率），进行语法转换。  
配置优先级 `targets`>`browserslist`

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": 3 // or 2
      }
    ]
  ]
}
```

##### .browserslistrc

配置代码运行**目标环境**。  

- `autoprefixer`、`postcss` 🉑根据 `browserslist` 自动判断是否要增加 `CSS` 前缀；
- 如果使用了 `@babel/preset-env` 预设，`Babel` 也会读取 `browserslist` 配置；

[图形化显示浏览器支持范围](https://browsersl.ist/)

##### useBuiltIns 选项

```ad-danger
When this plugin is enabled, the `useBuiltIns` option in `@babel/preset-env` must not be set. Otherwise, this plugin may not able to completely sandbox the environment.
```

```json
"entry": 须手动在主文件入口/构建工具入口处引入`@babel/polyfill`
"usage": 无须手动引入`@babel/polyfill`, Babel自动引入polyfill
false: 须手动在主文件入口/构建工具入口处引入`@babel/polyfill`，且被全部`whole`引入到最终的代码里❗️❗️❗️
```

`useBuiltIns: "entry"`  
须手动在主文件入口/构建工具入口处引入 `@babel/polyfill`；根据目标环境缺失 `API` 进行引入，不管代码中是否用到

```js
"use strict";

function _typeof(o) { "@babel/helpers - typeof"; return _typeof = "function" == typeof Symbol && "symbol" == typeof Symbol.iterator ? function (o) { return typeof o; } : function (o) { return o && "function" == typeof Symbol && o.constructor === Symbol && o !== Symbol.prototype ? "symbol" : typeof o; }, _typeof(o); }
require("core-js/modules/es6.array.copy-within.js");
require("core-js/modules/es6.array.fill.js");
require("core-js/modules/es6.array.filter.js");
require("core-js/modules/es7.array.flat-map.js");
require("core-js/modules/es6.array.from.js");
require("core-js/modules/es7.array.includes.js");
require("core-js/modules/es6.array.iterator.js");
require("core-js/modules/es6.array.map.js");
require("core-js/modules/es6.array.slice.js");
require("core-js/modules/es6.array.species.js");
require("core-js/modules/es6.date.to-primitive.js");
require("core-js/modules/es6.function.has-instance.js");
require("core-js/modules/es6.map.js");
require("core-js/modules/es6.math.clz32.js");
require("core-js/modules/es6.math.hypot.js");
require("core-js/modules/es6.number.constructor.js");
require("core-js/modules/es6.number.is-safe-integer.js");
require("core-js/modules/es6.number.max-safe-integer.js");
// …等等，还有非常多没用到API也被引入
```

`useBuiltIns: "usage"`  
无须手动引入 `@babel/polyfill`, `Babel` 自动引入 `polyfill`；代码中用到的 `API` 针对性引入 `pollyfill`  

```js
"use strict";

function _typeof(o) { "@babel/helpers - typeof"; return _typeof = "function" == typeof Symbol && "symbol" == typeof Symbol.iterator ? function (o) { return typeof o; } : function (o) { return o && "function" == typeof Symbol && o.constructor === Symbol && o !== Symbol.prototype ? "symbol" : typeof o; }, _typeof(o); }
require("core-js/modules/es6.symbol.js");
require("core-js/modules/es6.string.iterator.js");
require("core-js/modules/es6.array.iterator.js");
require("core-js/modules/web.dom.iterable.js");
require("core-js/modules/es7.symbol.async-iterator.js");
require("core-js/modules/es6.object.get-prototype-of.js");
require("core-js/modules/es6.object.set-prototype-of.js");
require("core-js/modules/es6.array.slice.js");
require("core-js/modules/es6.object.to-string.js");
require("core-js/modules/es6.promise.js");
require("core-js/modules/es6.object.assign.js");
// 没了，只用到这么些API
```

`useBuiltIns: false`  
产物中全量引入❗️❗️❗️

```js
"use strict";

require("@babel/polyfill");
// …
```

##### chorejs 选项

```ad-tip
`core-js`？
- `JavaScript` 标准化库的 `polyfill`；
- 模块化，按需引入；
- 与 `babel` 高度集成，最大程度优化部分引入而非全量引入；
```

`chorejs: 2` （默认值） `Babel` 转码的时候使用的是 `core-js@2` 版本  
`chorejs: 3` `Babel` 转码的时候使用的是 `core-js@3` 版本  
某些 `API` 如 `Array.prototype.flat` 只有 `core-js@3` 才有，只有配置为 `3` 才能将对应 `API` 补齐

##### modules 选项

```ad-tip
This option must be one of `'commonjs'`, `'amd'`, `'umd'`, `'systemjs'` - `'auto'` (default) which will automatically select `'false'` if the current process is known to support ES module syntax, or "commonjs" otherwise.(false - ES6, auto - commonjs)
```

### polyfill

```bash
pnpm add @babel/polyfill // 生产环境
```

使用方式：

1. 直接在 `html` 文件引入 `Babel` 官方的 `polyfill.js` 脚本文件 `<script src="https://xxx/polyfill.js"></script>`；
2. 在入口文件开头引入 `import './polyfill.js'`；
3. 在入口文件开头引入 `import '@babel/polyfill'`；
4. 在入口文件开头引入 `import "core-js/stable";import "regenerator-runtime/runtime";`；
5. 构建工具文件入口项配置引入，如 `webpack`： `entry: ['./polyfill.js', './index.js']`；
6. 构建工具文件入口项配置引入，如 `webpack`： `entry: ['@babel/polyfill', './index.js']`；
7. 构建工具文件入口项配置引入，如 `webpack`： `entry: ['core-js/stable', 'regenerator-runtime/runtime', './index.js']`；  

```ad-warning
We do not recommend that you import the whole polyfill directly, either try the `useBuiltIns` options or import only the polyfills you need manually
```

==副作用==：`7.4.0` 后不建议使用 `@babel/polyfill`，将在全局模拟完整的 `ECMA2015+` 功能，**造成污染**。而 `@babel/preset-env` 的 `"useBuiltIns": "usage"` 配置，只对使用到的、目标环境缺失的功能进行代码转换和添加 `polyfill`

### Tool Packages

#### @babel/runtime

is a library that contains Babel modular runtime helpers.

无需开启 `corejs2 API` 转换时，安装 `@babel/runtime` 即可；需要 `corejs3 API` 转换时需安装 `@babel/runtime-corejs2/3`

## 总结

`Babel` 三大功能：

- `only syntax transform`
- `pollyfill`  

其中 `pollyfill` 存在副作用：

- `re-use`
- `pollute global`

解决：

- `@babel/plugin-transform-runtime auto import @babel/runtime helper functions`
- `config chorejs config`

# Reference

[Babel 教程 - 姜瑞涛的官方网站](https://www.jiangruitao.com/babel/) 🎉🎉🎉  
[编译 ts 代码用 tsc 还是 babel？ - 掘金](https://juejin.cn/post/7084882650233569317)

[Babel相关内容串联 | Congzhou's Blog](https://congzhou09.github.io/knowledge/Babel%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9%E4%B8%B2%E8%81%94.html)  
[前端工程化（7）：你所需要知道的最新的babel兼容性实现方案 - 掘金](https://juejin.cn/post/6976501655302832159)  
[一文搞懂 core-js@3、@babel/polyfill、@babel/runtime、@babel/runtime-corejs3 的作用与区别 - 掘金](https://juejin.cn/post/7062621128229355528)

[一文彻底读懂Babel - 谢小飞的博客](https://xieyufei.com/2020/11/18/Babel-Practice.html)

[一文搞懂Babel配置 - 掘金](https://juejin.cn/post/7116698494827495454)  
[babel配置指南 · Issue #16 · zyl1314/blog · GitHub](https://github.com/zyl1314/blog/issues/16)  

[深入浅出 Babel 上篇：架构和原理 + 实战 - 掘金](https://juejin.cn/post/6844903956905197576?searchId=20230919153112D7757C501CCE1BA2EF5F)
