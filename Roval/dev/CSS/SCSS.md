
创建日期：2022-05-28 12:25:00
最后修改：2022-05-28 12:25:00
- - -
> Something opens our wings. Something makes boredom and hurt disappear. Someone fills the cup in front of us: We taste only sacredness.
> — <cite>Rumi</cite>

## 正文
CSS本身并不是一门编程语言，它更像是设计师的工具，对程序员来说十分不友好，为此出现了预处理器，为CSS开发过程中加入编程元素，并将SCSS文件编译为浏览器可以识别的CSS。

### 1. 使用变量
#### 1-1 变量声明
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

#### 1-2 变量引用
凡是 `css` 属性值存在的地方都可以用 `Sass` 变量赋值，编译结果中，变量会被替换为它的值。

```scss
$highlight-color: green;  
$highlight-border: 1px solid $highlight-color;
```

#### 1-3 变量名用_还是-分割
除纯 `css` 部分如类名外，`Sass` 中是互通的。

```scss
$bg-color: pink;  
.contain {
	background-color: $bg_color;  
}
```

### 2. 嵌套规则
嵌套规则可以避免`css`中大量重复书写工作，可以像俄罗斯🪆一样层层打开。

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

#### 2.1 父选择器标识符 `&`
默认情况下，`Scss` 在解开一层嵌套时会把父选择器通过一个空格加在子选择器前面，在 `CSS` 中含义为后代选择器，然而很多时候这种 `CSS` 后代选择器的方式并不满足需求，`Scss` 提供了 `&` 选择器为解开嵌套提供了更多机制。 添加伪类的用法：

```scss
a {
	text-decoration: none;  
	color: black;
	&:hover {
		color: cornflowerblue;  
	}  
}
```
在父选择器前加选择器的用法：
```scss
.txt-tip {
	font-size: 20px;
	.small-screen & {
		font-size: 16px;  
	}
	.big-screen & {
		font-size: 22px;
	}  
}
```

### 2.2 群组选择器嵌套
解 `CSS` 群组选择器
```scss
// from css  
.container h1, .container h2, .container h3 { margin-bottom: .8em; }  
// to scss  
.container {
	h1, h2, h3 {
		margin-bottom: .8em;
	}  
}  
// from scss  
nav, aside {  
 a {color: blue}  
}  
// to css  
nav a, aside a {  
 color: blue;  
}
```

### 6. SassScript
#### 6-1 Interactive Shell
通过命令行输入 `sass -i`  
## 参考文献
[Scss 官网](https://www.sass.hk/)