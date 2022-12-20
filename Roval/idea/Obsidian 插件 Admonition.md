Created Date：2022-12-18 23:05:28  
Last Modified：2022-12-18 23:05:27

# Tags

#Obsidian

# Content

## 用法示例

### 基础样式

```ad-note
Admonition 是提供块级样式风格的一个插件。
```

```ad-seealso
其他。
```

```ad-abstract
摘要。
```

```ad-summary
总结。
```

```ad-example
示例。
```

```ad-danger
危险。
```

```ad-bug
缺陷 bug。
```

```ad-success
成功。
```

```ad-check
检查。
```

```ad-done
完成。
```

```ad-caution
注意！
```

```ad-warning
警告。
```

```ad-attention
注意。
```

```ad-failure
失败。
```

```ad-fail
失败。
```

```ad-missing
缺失。
```

```ad-info
信息。
```

```ad-todo
待办清单。
```

```ad-faq
问答部分。
```

```ad-question
问题。
```

```ad-help
帮助。
```

```ad-cite
引用。
```

```ad-quote
引用。
```

```ad-hint
提示。
```

```ad-tip
提示。
```

```ad-important
重点。
```

### 更改默认样式

#### 自定标题

```ad-note
title: `JavaScript`
用 `title:` 改了标题为 JavaScript，支持 Markdown 语法。
```

#### 可折叠

```ad-faq
collapse: open
可折叠的 Anomition 。
```

#### 自定图标

```ad-info
title:  React
icon: react
可以改图标，但必须是 [fontAwesome](https://fontawesome.com/icons) 或 RPGAwesome 支持的。
```

```ad-info
title: 批注
icon: comment
哈哈哈。
```

#### 自定颜色

```ad-info
color: 200,200,200

```

```ad-info
color: 128,109,158
```

```ad-info
color: 255,201,12
通过 color 改颜色，输入 `color: R,G,B`
```

```ad-info
color: 231,124,142
```

#### 嵌套

``````ad-info
title: 4
`````ad-info
title: 3
````ad-info
title: 2
```ad-info
title:1
```
````
`````
``````

# Reference

1) [Github](https://github.com/valentine195/obsidian-admonition)
