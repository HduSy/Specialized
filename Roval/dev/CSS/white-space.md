# White-space

摘要：空白符及换行符处理。
双链：[[juejin-list#彻底搞懂word-break、word-wrap、white-space https juejin cn post 6844903667863126030|彻底搞懂word-break、word-wrap、white-space]]

```css
white-space: normal; // 默认值。连续的空白符会被合并，换行符会被当作空白符，文本换行
white-space: nowrap; // 连续的空白符会被合并，文本不换行
white-space: pre; // 保留所有空白符与换行符，代码里怎么写就怎么显示，但文本不换行
white-space: pre-wrap; // 保留所有空白符与换行符，代码里怎么写就怎么显示，但文本换行
white-space: pre-line; // 连续的空白符会被合并，换行符被保留
```

![[Pasted image 20220208150310.png]]  
需要注意的是，`<br>` 仍能正常换行
