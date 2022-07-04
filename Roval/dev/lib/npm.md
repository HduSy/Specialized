
创建日期：2022-03-03 14:30:08
最后修改：2022-03-03 14:30:08
- - -
> Accept challenges, so that you may feel the exhilaration of victory.
> — <cite>George S. Patton</cite>

## 正文
#### 见过的 npm 包
- 开发CLI脚手架必备
https://github.com/tj/commander.js
- Node 环境下 json 文件读写
https://github.com/jprichardson/node-jsonfile
- 终端字体样式设置
https://github.com/chalk/chalk
- 终端进度转轮提示
https://www.npmjs.com/package/ora
- 验证是否合法 npm 包名
https://github.com/npm/validate-npm-package-name
- 命令行交互
https://github.com/SBoudrias/Inquirer.js
- 下载一个 git repo
https://www.npmjs.com/package/download-git-repo
- css in js React 实践
https://styled-components.com/docs/basics CSS删除容易、无类名冲突
- px2Rem
https://www.npmjs.com/package/px2rem
- postcss flex 布局下 bug 修复补丁
https://www.npmjs.com/package/postcss-flexbugs-fixes
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
- zip 压缩方案
[压缩解压缩 zip 到本地disk or 内存 buffer](https://github.com/cthackers/adm-zip)
- 获取 .md 类型文件 MD5值
[md5-file](https://www.npmjs.com/package/md5-file)
- glog 语法快速遍历文件系统，支持模式匹配，返回文件路径名
[fast-glob](https://github.com/mrmlnc/fast-glob)
- 


## 参考文献
[2222 年了，总不能还只会 npm i 吧?🔥](https://juejin.cn/post/7069701706606444551)
