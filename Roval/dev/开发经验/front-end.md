一些开发中起一定奇特作用的函数

# 防抖截流 [[func]]

# 换行省略

```scss
@mixin multilineTextEllipsis($line, $line-height, $width) {
	width: $width;
	line-height: $line-height;
	min-height: $line-height;
	// height: $line-height * $line;
	overflow:hidden;  
	@if $line == 1 {  
		text-overflow:ellipsis;  
		white-space:nowrap;
	}
	@if $line > 1{
		display: -webkit-box;  
		-webkit-box-orient: vertical;  
		-webkit-line-clamp: $line;  
		box-orient: vertical;  
		line-clamp: $line;
	}
}
```

# 隐藏滚动条

```scss
@mixin hide-scrollbar {  
  scrollbar-width: none;  
  scrollbar-color: transparent transparent;  
  &::-webkit-scrollbar {  
    display: none;  
    background-color: transparent;  
  }  
  &::-webkit-scrollbar-thumb {  
    background-color: transparent;  
  }  
  &::-webkit-scrollbar-track {  
    background-color: transparent;  
  }  
}
```

# 自定义滚动条样式

## 参考链接

[::-webkit-scrollbar - CSS：层叠样式表 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::-webkit-scrollbar)

## 上代码

```scss
@mixin scroll-self {
	overflow-x: hidden;
	overflow-y: scroll;
	overscroll-behavior: contain;
	overflow: overlay;
	// 整个滚动条
	&::-webkit-scrollbar {
		width: 5px;
	}
	// 滚动条滑轨
	&::-webkit-scrollbar-track {
		background-color: rgba($color: #000000, $alpha: 0);
	}
	// 滚动条滑块
	&::-webkit-scrollbar-thumb {
		background-color: rgba($color: #fff, $alpha: 0.5);
		border-radius: 3px;
	}
}
```
