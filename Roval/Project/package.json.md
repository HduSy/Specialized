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
