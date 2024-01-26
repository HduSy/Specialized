Created Date：2023-09-23 21:02:51  
Last Modified：2023-09-23 21:02:50

# Tags

# Content

## type

指明文件为哪种模块处理方式。`require/module.exports` 的 `Node commonjs` 模块还是 `import/export` 的 `ECMAScript module` 模块处理。

三点说明：  
1、建议始终不要忽略，虽然**默认**是 `commonjs` 规范；  
2、指明了 `.js` 和无扩展名文件处理方式；  
3、不受 `type` 影响的两种类型文件，`.mjs` 的文件都按照 `ES` 模块来处理，`.cjs` 的文件都按照 `commonJs` 模块来处理；

## files

（可选）文件数组，列出安装该依赖时会包括的条目，忽略时将包含所有文件。

```
以下文件无论是否设置，总是包含：

package.json
README
CHANGES/CHANGELOG/HISTORY
LICENSE/LICENCE
NOTICE

以下文件总是被忽略：
.git
CVS
.svn
.hg
.lock-wscript
.wafpickle-N
.*.swp
.DS_Store
._*
npm-debug.log
.npmrc
node_modules
config.gypi
*.orig
package-lock.json
```

## main

（可选）主入口文件，如 `.dist/src/index.js`

## sideEffects

> The new webpack 4 release expands on this capability with a way to provide hints to the compiler via the `"sideEffects"` `package.json` property to denote which files in your project are "pure" and therefore safe to prune if unused.

类型 `boolean|string[]`，非 `package.json` 官方，`webpackv4` 新增属性，为了配合 `tree-shaking` 进行打包优化（节省代码体积、减少对源码的分析，加快打包速度）。  

`webpack` 打包时默认会将虽未使用但有副作用的代码保留下来，**该项配置 `tree-shaking` 时对有副作用文件/代码的处理程度，保留 or 移除**

```ad-warning
`webpack` 会认为所有 `import 'xxx'` 语句是仅引入而未使用, 如果你错误的将其声明成了”无副作用”, 它们就会被`tree-shaking` 掉, 并且由于 `tree-shaking` 仅在 `production` 模式生效, 本地开发时可能一切仍是正常的, 生产环境并不能及时发现问题.
```

### 自测

```js
// a.js 无副作用
export const str = 'Hello Webpack'

// b.js 无副作用
function print(str) {
  console.log(str)
}
console.log('console有副作用')
export { print }

// index.js 打包入口文件
import { str } from './src/a'
import { print } from './src/b'
import './src/css/base.css'
console.log(str)
```

#### `sideEffects: true`

等效于不设置该项。打包结果包含仅引入但未使用的 `print` 函数

```js
// ...
function print(str) {\n console.log(str)\n}\n\nconsole.log('console有副作用')
// ...
```

#### `sideEffects: false`

关闭副作用检查。整个模块/包没有副作用，尽情地把没引用/用到的代码 `shaking` 掉。  

打包结果不包含 `print` 函数相关代码，同时也没了样式文件 `base.css`，样式无效

```js
// 引入未使用，配置了false，导致样式失效
import './normalize.css';  
import './polyfill';  
import './App.less';
```

#### `sideEffects: ["./src/**/*.css"]`

指定哪些文件含副作用代码，这些文件无论是否使用都不会被 `shaking` 掉

```js
// 样式引入
...css-loader@6.9.1_webpack@5.90.0/node_modules/css-loader/dist/cjs.js!./base.css */ \"../../node_mo...
// js模块引入
...y export */ });\nconst aaa = 'aaa'\n\n\n//#...
```

### 局限

`sideEffects` 配置是以文件为维度的, 只要你配置了文件具备副作用, 即便你只用了该文件中没有副作用的那部分功能, 仍然会将副作用保留  

### 参考

[深入理解sideEffects配置 | 明日之事 事事难求](https://libin1991.github.io/2019/05/01/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3sideEffects%E9%85%8D%E7%BD%AE/)

## exports

优先级比 `main` 高，包含了多种导出形式： 默认导出、子路径导出和条件导出

```json
// package.json
{
  "name": "package-a",
  "type": "module", // 指定ESM模块规范
  "exports": {
    // 默认导出，使用方式: import a from 'package-a'
    ".": "./dist/index.js",
    // 子路径导出，使用方式: import d from 'package-a/dist'
    "./dist": "./dist/index.js",
    "./dist/*": "./dist/*", // 这里可以使用 `*` 导出目录下所有的文件
    // 条件导出，区分 ESM 和 CommonJS 引入的情况
    "./main": {
      "import": "./main.js",
      "require": "./main.cjs"
    },
  }
}
```

- `node`：在 `Node.js` 环境下适用，可以定义为嵌套条件导出
- `import`：用于 `import` 方式导入的情况，如 `import("package-a")`;
- `require`：用于 `require` 方式导入的情况，如 ` require("package-a")`;
- `default`：兜底方案，如果前面的条件都没命中，则使用 `default` 导出的路径。  
条件导出还包含 `types、browser、develoment、production` 等属性

## browser

当依赖作为客户端浏览器使用时，应使用 `browser` 替代 `main`，告诉用户可能包含 `node` 环境不支持的用法。

## bin

提供的内部命令对应可执行文件位置。

## private

`npm publish` 拒绝发布，防止意外发布到开源社区。

## repository

代码仓库地址。

## scripts

命令行 `npm` 脚本缩写。

## config

添加命令行环境变量。

## dependencies

通过 `npm install --save` 命令安装依赖。

`Please do not put test harnesses or transpilers or other "development" time tools in your dependencies object.`  

不要把测试、开发用依赖安装到 `dependencies`。

## devDependencies

通过 `npm run install --save-dev` 命令安装依赖。

## engines

项目以来 `node` 版本。

## package.lock.json 作用

## 总结

1. 锁定版本号，始终保证各团队成员、CI 安装同一份完全相同的依赖树；
2. 通过 Git 版本管理工具，清晰查看 diff 从而掌握依赖树变动；
3. 不必提交整个 node_modules 文件夹，通过 packge.lock.json 即可保证依赖树相同；
4. 跳过已安装包，优化安装过程。

# Reference

[npm 依赖管理中被忽略的那些细节_语言 & 开发_政采云前端团队_InfoQ精选文章](https://www.infoq.cn/article/qj3z2ygrzdgicqauaffn)
