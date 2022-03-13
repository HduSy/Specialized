
创建日期：2022-03-01 21:21:23
最后修改：2022-03-01 21:21:23
- - -
> Nothing diminishes anxiety faster than action.
> — <cite>Walter Inglis Anderson</cite>

## 正文
### font-minify
![[Pasted image 20220301212244.png]]
### index.js
```js
const Fontmin = require('fontmin')  
const path = require('path')  
const rename = require('gulp-rename')  
  
let text = '测试' // 提取文字
function minifyFont() {  
  const fontmin = new Fontmin()  
    .src(path.join(__dirname, './raw-font.ttf')) // 输入配置  
 .use(rename('font.ttf'))  
    .use(Fontmin.glyph({ // 字型提取插件  
 text: text, // 所需文字  
 }))  
    // .use(rename('font-compressed.ttf'))  
 .use(Fontmin.ttf2eot()) // eot 转换插件  
 .use(Fontmin.ttf2woff()) // woff 转换插件  
 .use(Fontmin.ttf2woff2()) // woff2 转换插件  
 .use(Fontmin.ttf2svg()) // svg 转换插件  
 .use(Fontmin.css()) // css 生成插件  
 .dest(path.join(__dirname, './'))  
  fontmin.run(function(err, files, stream) {  

    if (err) { // 异常捕捉  
 console.error(err)  
    }  
    console.log('done') // 成功  
 })  
}  
minifyFont()
```
## 参考文献