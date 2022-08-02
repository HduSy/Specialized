创建日期：2022-03-03 14:30:08  
最后修改：2022-03-03 14:30:08

#前端 #cli

- - -
> Accept challenges, so that you may feel the exhilaration of victory.  
>—<cite>George S. Patton</cite>

# 正文

## 配置文件说明 package.json

### Type

指明文件为哪种模块处理方式。`require/module.exports` 的 `Node commonjs` 模块还是 `import/export` 的 `ES6` 模块处理。

### Files

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

### Main

（可选）主入口文件，默认 `index.js`。

### Browser

当依赖作为客户端浏览器使用时，应使用 `browser` 替代 `main`，告诉用户可能包含 `node` 环境不支持的用法。

### Bin

提供的内部命令对应可执行文件位置。

### Private

`npm publish` 拒绝发布，防止意外发布到开源社区。

### Repository

代码仓库地址。

### Scripts

命令行 `npm` 脚本缩写。

### Config

添加命令行环境变量。

### Dependencies

通过 `npm install --save` 命令安装依赖。

`Please do not put test harnesses or transpilers or other "development" time tools in your dependencies object.`  

不要把测试、开发用依赖安装到 `dependencies`。

### devDependencies

通过 `npm run install --save-dev` 命令安装依赖。

### Engines

项目以来 `node` 版本。

## 见过的 Npm 包

开发 CLI 脚手架必备 [commander.js](https://github.com/tj/commander.js)

Node 环境下 Json 文件读写 [node-jsonfile](https://github.com/jprichardson/node-jsonfile)

终端字体样式设置 [chalk](https://github.com/chalk/chalk)

终端进度转轮提示 [ora](https://www.npmjs.com/package/ora)

验证是否合法 Npm 包名 [validate-npm-package-name](https://github.com/npm/validate-npm-package-name)

命令行交互 [inquirer](https://github.com/SBoudrias/Inquirer.js)

下载一个 Git Repo [download-git-repo](https://www.npmjs.com/package/download-git-repo)

CssInJsReact 实践 [styled-components](https://styled-components.com/docs/basics)

[px2rem](https://www.npmjs.com/package/px2rem)

Postcss Flex 布局下 Bug 修复补丁 [postcss-flexbugs-fixes](https://www.npmjs.com/package/postcss-flexbugs-fixes)

```js postcss.config.js
module.exports = {  
  plugins: [  
    require('autoprefixer')(),  
    require('postcss-flexbugs-fixes')(),  
    require('postcss-px2rem')({  
      remUnit: 100,  
    }),  
  ],  
};
```

Zip 压缩方案 [压缩解压缩 zip 到本地disk or 内存 buffer](https://github.com/cthackers/adm-zip)

获取 .md 类型文件 MD5 值 [md5-file](https://www.npmjs.com/package/md5-file)

Glob 语法快速遍历文件系统，支持模式匹配，返回文件路径名 [fast-glob](https://github.com/mrmlnc/fast-glob)

使用 Glob 语法删除文件/文件夹 [del](https://www.npmjs.com/package/del)

VSCODE 代码编辑器 [monaco-editor](https://github.com/microsoft/monaco-editor)

ORM 框架，方便操作数据库 [typeorm](https://typeorm.bootcss.com/)

Nodejs 压缩中间件 [compression](https://www.npmjs.com/package/compression)

Nodejs Cookie 中间件 [cookie-parser](https://www.npmjs.com/package/cookie-parser)

可读性好的 Api 创建 Schema 描述文件验证对象结构 [joi](https://joi.dev/)

普通对象与类实例间相互转换 [class-transformer](https://www.npmjs.com/package/class-transformer)

基于装饰器的类型验证 [class-validator](https://www.npmjs.com/package/class-validator)

React Router [react-router-dom](https://v5.reactrouter.com/web/guides/quick-start)

## 升级

### Npm

#### 描述

几次大版本升级。

#### 提升

无。

#### 命令

npm link 本地开发测试依赖包免构建发版流程

### Yarn

### Pnpm

#### 描述

#### 提升

1、优化磁盘空间  
2、大幅提升下载速度

#### 命令

##### 本地免发布调试开发中的 Npm 包

`pnpm link <dir>`：指定 `dir` 目录下软件包链接到当前目录下 `node_modules` 目录中；

`pnpm link --global`：将当前工作目录或 `--dir` 参数指定目录下软件包链接到全局环境 `node_modules` 目录中；

`pnpm link --global [pkg-name]`：将全局环境 `node_modules` 目录中指定软件包链接到当前工作目录下；

## 参考文献

1. [2222 年了，总不能还只会 npm i 吧?🔥](https://juejin.cn/post/7069701706606444551)  
2. [前端工程化之强大的glob语法](https://juejin.cn/post/6876363718578405384)
3. [pnpm使用详细纤细说明/浏览26301/点赞361](https://juejin.cn/post/7053340250210795557)  
4. [npm 官网关于 package.json 详细字段说明](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)
5. [pnpm 中文官网](https://www.pnpm.cn/)
