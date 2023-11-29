Created Date：2022-12-17 20:56:17  
Last Modified：2022-12-17 20:56:17

# Tags

#CSS

# Content

空白符及换行符处理

```css
white-space: normal; // 默认值。连续的空白符会被合并，换行符会被当作空白符，文本换行
white-space: nowrap; // 连续的空白符会被合并，文本不换行
white-space: pre; // 保留所有空白符与换行符，代码里怎么写就怎么显示，但文本不换行
white-space: pre-wrap; // 保留所有空白符与换行符，代码里怎么写就怎么显示，但文本换行
white-space: pre-line; // 连续的空白符会被合并，换行符被保留
```

![[Pasted image 20220208150310.png]]  
需要注意的是，`<br>` 仍能正常换行

# Reference

[彻底搞懂word-break、word-wrap、white-space](https:juejin.cn/post/6844903667863126030)