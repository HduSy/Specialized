一些开发中起一定奇特作用的函数
##### 防抖截流[[func]]
##### 换行省略
```scss
@mixin multilineTextEllipsis($line, $line-height, $width) {
	width: $width;
	line-height: $line-height;
	height: $line-height * $line;  
	@if $line == 1 {
		overflow:hidden;  
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



