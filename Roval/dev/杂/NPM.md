Created Date：2023-04-04 10:04:47  
Last Modified：2023-04-04 10:04:46

# Tags

#前端工程化  

# Content

## 配置文件说明 package.json

### main

（可选）主入口文件，默认 `index.js`。

### type

指明项目中的 `.js` 文件以哪种模块处理。`require/module.exports` 的 `Node commonjs` 模块还是 `import/export` 的 `ECMAScript module` 模块处理。

三点说明：  
1、建议始终不要忽略，参数缺失时**默认**是 `commonjs` 规范；  
2、指明了 `.js` 和无扩展名文件处理方式；  
3、不受 `type` 影响的两种类型文件，`.mjs` 的文件都按照 `ES` 模块来处理，`.cjs` 的文件都按照 `commonJs` 模块来处理；

### exports

[package.json 的 exports 字段](https://es6.ruanyifeng.com/#docs/module-loader#package-json-%E7%9A%84-exports-%E5%AD%97%E6%AE%B5)

### files

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

### browser

当依赖作为客户端浏览器使用时，应使用 `browser` 替代 `main`，告诉用户可能包含 `node` 环境不支持的用法。

### bin

提供的内部命令对应可执行文件位置。

### private

`npm publish` 拒绝发布，防止意外发布到开源社区。

### repository

代码仓库地址。

### scripts

命令行 `npm` 脚本缩写。

### config

添加命令行环境变量。

### dependencies

通过 `npm install --save` 命令安装依赖。

`Please do not put test harnesses or transpilers or other "development" time tools in your dependencies object.`  

不要把测试、开发用依赖安装到 `dependencies`。

### devDependencies

通过 `npm run install --save-dev` 命令安装依赖。

### engines

项目依赖 `node` 版本。

## package.lock.json 作用

[npm 依赖管理中被忽略的那些细节_语言 & 开发_政采云前端团队_InfoQ精选文章](https://www.infoq.cn/article/qj3z2ygrzdgicqauaffn)

### 总结

1. 锁定版本号，始终保证各团队成员、CI 安装同一份完全相同的依赖树；
2. 通过 Git 版本管理工具，清晰查看 diff 从而掌握依赖树变动；
3. 不必提交整个 node_modules 文件夹，通过 packge.lock.json 即可保证依赖树相同；
4. 跳过已安装包，优化安装过程。

## npm-cli

### npm-exec

Run a command from a local or remote npm package.

# Reference

[2222 年了，总不能还只会 npm i 吧?🔥](https://juejin.cn/post/7069701706606444551)  
[npm 官网关于 package.json 详细字段说明](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)

[Node.js 如何处理 ES6 模块 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2020/08/how-nodejs-use-es6-module.html)  
[ES6 入门教程](https://es6.ruanyifeng.com/#docs/module-loader)
