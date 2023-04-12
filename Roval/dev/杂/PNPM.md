Created Date：2023-04-04 10:05:29  
Last Modified：2023-04-04 10:05:29

# Tags

#前端工程化

# Content

## Pnpm

### 介绍

#### 项目初衷

节省磁盘空间；提升安装速度。

- 依赖被安装在基于内容寻址的存储中，不存在多个重复拷贝；
- 依赖项不同版本的包，只会将差异文件添加到仓库；
- 依赖文件会存储到磁盘某一位置，安装已有依赖会硬链接到这一位置，不占用额外磁盘空间。

#### 创建非扁平化 node_modules 目录结构

诀窍一：node 会解析符号连接，如 `.pnpm/express@4.17.1/node_modules/express` 去寻找依赖真正所在位置。`.pnpm/` 文件夹（虚拟存储目录）中平铺着依赖，解决了 `npm v2` 依赖长路径问题，同时避免 `npm v3+` 及 `yarn v1` 安全问题，保留了包之间的隔离；

诀窍二：依赖包与依赖项的实际位置位于同一目录级别，比如 `express` 依赖项都存放在 `.pnpm/express@4.17.1/node_modules` 下，与 `express` 同级，避免了循环软链。

> [!info]+  
`peer` 依赖会特殊点处理…

#### 基于符号链接的 node_modules 结构

pnpm 的 `node_modules` 布局使用符号链接来创建依赖项的嵌套结构，只有真正在依赖项中的包才能访问。当你安装依赖后，pnpm 将在 `node_modules` 创建依赖对应硬链接。

### 描述

### 提升

1、优化磁盘空间  
2、大幅提升下载速度

### 命令

#### 等同于 Npx 功能

`pnpm dlx <pkg_name> [options]`

#### 本地免发布调试开发中的 Npm 包

`pnpm link <dir>`：指定 `dir` 目录下软件包链接到当前目录下 `node_modules` 目录中；

`pnpm link --global`：将当前工作目录或 `--dir` 参数指定目录下软件包链接到全局环境 `node_modules` 目录中；

`pnpm link --global [pkg-name]`：将全局环境 `node_modules` 目录中指定软件包链接到当前工作目录下；

# Reference

[pnpm 中文官网](https://www.pnpm.cn/)  
[pnpm使用详细纤细说明/浏览26301/点赞361](https://juejin.cn/post/7053340250210795557)  
[一个pnpm安装express目录结构的参考](https://github.com/zkochan/comparing-node-modules/tree/master/pnpm5-example)
