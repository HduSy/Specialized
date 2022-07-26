创建日期：2022-05-22 21:33:51  
最后修改：2022-05-22 21:33:51

- - -
> Real magic in relationships means an absence of judgement of others.  
>—<cite>Wayne Dyer</cite>

# 正文

## 关于

`JavaScript` 是一门动态的弱类型语言，开发中非常容易出错，想要调试必须让代码执行起来。`eslint` 是一种代码检查工具，提供可插入的规则，让开发者在编程过程尽早发现有问题的模式和代码，从而保证代码的一致性和避免错误。

## 配置文件的存在形式

`package.json` 的 `eslintConfig` 属性 和 `.eslintrc.*`（`.eslintrc.js`、`.eslintrc.yml`、`.eslintrc.yaml`、`.eslintrc.json`），使用优先级：

1. `.eslintrc.js`
2. `.eslintrc.yaml`
3. `.eslintrc.yml`
4. `.eslintrc.json`
5. `.eslintrc`
6. `package.json`

### 配置优先级

#### 就近原则

默认情况下，`eslint` 会在所有父级目录下寻找配置文件，就近使用配置文件，且近者优先，组合并覆盖远的配置。有些特殊情况想把 `eslint` 限定到特定目录，可在配置文件中添加如下配置项：

```
{
    "root": true
}
```

完整的配置层次结构，从最高优先级最低的优先级，如下:

#### 优先级

1. 行内配置  
		1. `/*eslint-disable*/` 和 `/*eslint-enable*/`  
		2. `/*global*/`  
		3. `/*eslint*/`  
		4. `/*eslint-env*/`
2. 命令行选项（或 CLIEngine 等价物）：
		1. `--global`  
		2. `--rule`  
		3. `--env`  
		4. `-c`、`--config`
3. 项目级配置：
		1. 与要检测的文件在同一目录下的 `.eslintrc.*` 或 `package.json` 文件  
		2. 继续在父级目录寻找 `.eslintrc` 或 `package.json` 文件，直到根目录（包括根目录）或直到发现一个有 `"root": true` 的配置。
4. 如果不是（1）到（3）中的任何一种情况，退回到 `~/.eslintrc` 中自定义的默认配置。

## 原理

## 高级配置

### 解析器 - Parser

`eslint` 默认使用 `espree` 解析器，通过该配置项指定其他解析器，要求：

- 一个 `Node` 模块，必须通过 `npm` 单独安装解析器依赖；
- 必须符合 [parser-interface](https://cn.eslint.org/docs/developer-guide/working-with-plugins#working-with-custom-parsers)

### 解析器选项 - parserOptions

解析器配置项。

```json
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "semi": "error"
    }
}
```

### 处理器 - Processer

### 环境 - Environments

定义一组预定义的全局变量。

- `browser` - 浏览器环境中的全局变量。
- `node` - Node.js 全局变量和 Node.js 作用域。
- `commonjs` - CommonJS 全局变量和 CommonJS 作用域 (用于 Browserify/WebPack 打包的只在浏览器中运行的代码)。
- `shared-node-browser` - Node.js 和 Browser 通用全局变量。
- `es6` - 启用除了 modules 以外的所有 ECMAScript 6 特性（该选项会自动设置 `ecmaVersion` 解析器选项为 6）。
- `worker` - Web Workers 全局变量。
- `amd` - 将 `require()` 和 `define()` 定义为像 [amd](https://github.com/amdjs/amdjs-api/wiki/AMD) 一样的全局变量。
- `mocha` - 添加所有的 Mocha 测试全局变量。
- `jasmine` - 添加所有的 Jasmine 版本 1.3 和 2.0 的测试全局变量。
- `jest` - Jest 全局变量。
- `phantomjs` - PhantomJS 全局变量。
- `protractor` - Protractor 全局变量。
- `qunit` - QUnit 全局变量。
- `jquery` - jQuery 全局变量。
- `prototypejs` - Prototype.js 全局变量。
- `shelljs` - ShellJS 全局变量。
- `meteor` - Meteor 全局变量。
- `mongo` - MongoDB 全局变量。
- `applescript` - AppleScript 全局变量。
- `nashorn` - Java 8 Nashorn 全局变量。
- `serviceworker` - Service Worker 全局变量。
- `atomtest` - Atom 测试全局变量。
- `embertest` - Ember 测试全局变量。
- `webextensions` - WebExtensions 全局变量。
- `greasemonkey` - GreaseMonkey 全局变量。

### 全局变量 - Globals

### 插件 - Plugins

每个插件都是一个命名为 `eslint-config-<plugin-name>` 的 `npm` 模块，暴露额外规则以供使用。在配置 `plugins` 属性值时可省略前缀。

### 规则 - Rules

#### 规则禁用

禁用规则或禁用整个文件的规则校验：

```
/* eslint-disable [rulename,] */
```

禁用指定行：

```
alert('foo'); // eslint-disable-line [rulename,]
```

或

```
// eslint-disable-next-line [rulename,]
alert('foo');
```

还有能力通过配置 `overrides` 禁用指定文件：

```
{
  "rules": {...},
  "overrides": [
    {
      "files": ["*-test.js","*.spec.js"],
      "rules": {
        "no-unused-expressions": "off"
      }
    }
  ]
}
```

### 共享配置 - Settings

共享配置会提供给每一个将被执行的规则，发布为一个 npm 包。[创建共享配置](https://cn.eslint.org/docs/developer-guide/shareable-configs)

### 继承 - Extends

规则可以被继承。`extends` 属性值可以是：

1. 指定配置的字符串（配置文件路径、共享配置名称、`eslint:recommended` 或 `eslint:all`）
2. 字符串数组，后面的覆盖前面的

### eslint:recommended

[`eslint`推荐规则](https://cn.eslint.org/docs/rules/)

### eslint:all

不推荐使用。将开启所有核心规则，这些规则会随 `eslint` 版本迭代改变，源码未变时，将是破坏性的更改。

### .eslintignore

以 glob 匹配模式告诉 `eslint` 忽略指定文件夹和文件。

## Root

[定义指定目录下Eslint规则](https://eslint.org/docs/user-guide/configuring/configuration-files#cascading-and-hierarchy)  
默认情况下，ESLint 从父级目录里寻找配置文件，一直到根目录，如果你想要整个项目都遵循统（同）一约定时，这将会很有用。但另一种情况是把 ESLint 限制到一个特定的项目，这就需要在特定项目目录下的 `package.json` 文件或者 `.eslintrc.*` 文件里的 `eslintConfig` 字段下设置 `"root": true`。ESLint 一旦发现配置文件中有 `"root": true`，它就会停止在父级目录中寻找。

```js
{
	"root": true,
}
```

## Env

[定义支持的环境变量](https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments)

```js
{
	"env": {
		"browser": true,
		"node": true
    }
}
```

## Extend

[拓展"第三方"Eslint配置](https://eslint.org/docs/user-guide/configuring/configuration-files#extending-configuration-files)  
值可以是，相对/绝对路径下的配置文件、npm 包中的配置或 plugin 形式的配置

```js
extends: [  
	'plugin:vue/base',  
	'@bilibili/recommend', // bilibili代码规范  
	'@vue/typescript/recommended',  
],
```

## parserOptions

[指定支持的JavaScript版本](https://eslint.org/docs/user-guide/configuring/language-options#specifying-parser-options)

```js
parserOptions: {  
  ecmaVersion: 2020,  
},
```

## tsconfig.json

1、is declared but its value is never read.

```json
"compilerOptions": {
	"noUnusedLocals": false,
}
```

2、

## 代码文件里的临时规则设定

[当前文件关闭、当前行关闭等](https://eslint.bootcss.com/docs/user-guide/configuring#disabling-rules-with-inline-comments)

```
/* eslint-disable */
/* eslint-disable-line */
/* eslint-disable-next-line */
```

## 参考文献

- [Configuring Eslint](https://eslint.bootcss.com/docs/user-guide/configuring)
