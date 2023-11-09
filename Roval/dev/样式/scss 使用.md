创建日期：2022-05-28 12:25:00  
最后修改：2022-05-28 12:25:00

# Tags

#scss

# Content

CSS 本身并不是一门编程语言，它更像是设计师的工具，对程序员来说十分不友好，为此出现了预处理器，为 CSS 开发过程中加入编程元素，并将 SCSS 文件编译为浏览器可以识别的 CSS。

## 1 使用变量

### 1-1 变量声明

`Sass` 允许将 `css` 属性值定义为变量，方便在其他地方重复使用，符号为 `$`。

```scss
$black-color: #000;  
$one-red-solid-border: 1px solid red;  
$self-font: 'Specialized'、'Trek';  
$width: 100px;  
.rect {  
 width: $width;  
 height: $width;  
 border: $one-red-solid-border;  
}
```

### 1-2 变量引用

凡是 `css` 属性的标准值存在的地方都可以用 `Sass` 变量赋值，编译结果中，变量会被替换为它的值。

```scss
$highlight-color: green;  
$highlight-border: 1px solid $highlight-color;
```

当变量声明在规则块中，则变量只能在该规则块中使用。

### 1-3 变量名用 _ 还是 - 分割

除纯 `css` 部分如类名外，`Sass` 中是互通的。

```scss
$bg-color: pink;  
.contain {
	background-color: $bg_color;  
}
```

## 2 嵌套规则

嵌套规则可以避免 `css` 中大量重复书写工作，可以像俄罗斯🪆一样层层打开。

```scss
// from  
#content { 
	article {  
		h1 { color: #333 }  
		p { margin-bottom: 1.4em }  
	}
	aside { background-color: #EEE }  
}  
// to  
#content article h1 { color: #333 }  
#content article p { margin-bottom: 1.4em }  
#content aside { background-color: #EEE }
```

容器元素的规则会被单独抽离出来，而嵌套元素的样式则会像容器元素不含任何属性那样抽离

### 2.1 父选择器标识符 &

默认情况下，`Sass` 在解开一层嵌套时会**把父选择器通过一个空格加在子选择器前面**，在 `CSS` 中含义为**后代选择器**，这种默认的拼接方式有时并不满足需求，`Sass` 专门提供了 `&` 父选择器为解开嵌套提供了更多机制

添加伪类的用法：

```scss
// from
article a {
  color: blue;
  &:hover { color: red }
}
// to 并不会像默认方式一个简单空格拼接，而是将 `&` 直接替换为父选择器
article a { color: blue }
article a:hover { color: red }
```  

父选择器前添加父选择器的用法：

```scss
// from
#content aside {
  color: red;
  body.ie & { color: green }
}
// to
#content aside {color: red};
body.ie #content aside { color: green }
```

### 2.2 群组选择器嵌套 ,

`CSS` 群组选择器，命中任一选择器的元素生效（`,` 类比与 `或` 的概念）

```scss
// from scss  
.container {
	h1, h2, h3 {
		margin-bottom: .8em;
	}  
}
// from css  
.container h1, .container h2, .container h3 { margin-bottom: .8em; }  
// from scss  
nav, aside {  
 a {color: blue}  
}  
// to css  
nav a, aside a {  
 color: blue;  
}
```

### 2.3 CSS 选择器应用 空格>+~

[[css]]

```scss
// from
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
// to
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```

[CSS关系选择器实践 - CodeSandbox](https://codesandbox.io/s/cssguan-xi-xuan-ze-qi-shi-jian-wsljzw)

### 嵌套属性

```scss
// from
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
// to
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

## 3 导入 Sass 文件

可读性&可维护性  
原生 `CSS` 的 `@import` 只有当执行到时才会去下载，这样会导致页面加载缓慢。而 `sass` 的 `@import` **命令会在生成 css**的阶段把相关样式合并到同一个 `css` 文件，且定义的变量和混入都是可用的。

### Sass 局部文件

命名规范：`_` 开头，引入时可省略文件后缀 `.scss/.sass`。局部文件不会单独生成 `css` 文件，专门用来 `@import` 命令使用  

导入局部文件 `themes/_night-sky.scss` 写法：

```scss
@import` `"themes/night-sky";
```

### 默认变量值 !default

```scss
$fancybox-width: 400px !default;
```

### 嵌套导入

`sass` 的 `@import` 还有规则内导入的功能，不影响全局  
假如有局部样式文件 `_blue-theme.scss`：

```scss
aside {
  background: blue;
  color: white;
}
```

然后导入规则内：

```scss
.blue-theme {
	@import "blue-theme"
}
//生成的结果跟你直接在.blue-theme选择器内写_blue-theme.scss文件的内容完全一样。
.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
```

### 原生 css 的导入

触发条件：导入一个 `.css` 后缀的文件，就会让 `sass` 以为 `@import` 是原生的 css 导入

## 4 静默注释

```scss
body {  
  color: #333; // 这种注释内容不会出现在生成的 css 文件中  
  padding: 0; /* 这种注释内容会出现在生成的 css 文件中 */  
}
```

## 5 混入

`@mixin` 声明 `@include` 引用

### 使用场景

如果很容易能为一组属性想出一个简短的名字去描述，那就容易写一个合适的 `minxin`

### 仍适用 CSS 规则

支持选择器嵌套、支持父选择器标识符

### 混合器传参

忘记参数顺序时，也支持 `key-value` 方式传参

```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
// from
a {
  @include link-colors($normal: blue,$visited: green,$hover: red);
}
// to
a { color: blue; }
a:hover { color: green; }
a:visited { color: red; }
```

### 默认参数

## 选择器继承

`@extend`，继承另一个选择器定义的所有样式

## 7 SassScript

### Interactive Shell

通过命令行输入 `sass -i` 进行一些 `Sass` 支持的简单运算～

## 实战

# Reference

[scss 官网](https://www.sass.hk/)
