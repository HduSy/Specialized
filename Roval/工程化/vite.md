Created Date：2023-09-23 16:47:11  
Last Modified：2023-09-23 16:47:10

# Tags

#工程化 #前端工程化

# Content

## 依赖预构建

### 原因一 模块系统兼容性

`vite` 开发服务器只能识别 `ES` 模块源码，而第三方模块导出的模块系统如 `Commonjs`、`UMD` 源码是识别不了的，因此要进行转换，利用 `esbuild`

### 原因二 加载性能

将 `ES` 模块内部众多相互引用模块（如 `lodash-es`，会导致非常多的 `HTTP` 请求！），按需**预构建**成单个模块，之后只需一个 `HTTP` 请求！

### 缓存

存放目录：`node_modules/.vite`，以下任一项发生变化时引起重新构建：

1. 包管理器的锁文件；
2. `vite.config.js` 相关字段；
3. `NODE_ENV`；
4. 补丁文件修改；
5. `--force`

## 环境变量和模式

# Reference

[Vite | 下一代的前端工具链](https://cn.vitejs.dev/)
