# Word-break

摘要：到达行尾时的单词拆分表现。Chinese/Japanese/Korean (CJK)  
双链：[[juejin-list#彻底搞懂word-break、word-wrap、white-space https juejin cn post 6844903667863126030|彻底搞懂word-break、word-wrap、white-space]]

```css
word-break: normal; // 默认值，不拆分单个词。
word-break: break-all; // 避免溢出，碰到边界时，在任意字符间拆分换行
word-break: keep-all; // CJK并不会拆分词，非CJK表现如normal
```
