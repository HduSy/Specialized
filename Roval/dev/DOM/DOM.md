Created Date：2022-12-17 21:31:14  
Last Modified：2022-12-17 21:31:14

记录一些 HTML 学习小点，因为本身 HTML 元素及属性就多而无逻辑可言，且更新较快，不可能记着所有；另外一些实践中遇到的问题对应解决方案对于文档本身理解也是极其珍贵的。

# Tags

#HTML

# Content

## HTML 简介

概述：HTML 是网页的语言，描述网页内容与结构，从服务器下载 HTML 文件，浏览器将网页渲染出来。  
基本概念：网页由各种各样的 **标签** 构成，浏览器会渲染标签内容展示出来。浏览器在渲染网页的时候，会把 HTML 源码解析成一个标签树，每个标签都是一个节点，称为 **元素**（Element），标签是针对于 HTML 结构的叫法，节点是针对于编程上的叫法，两者是同义词。**属性** 是标签的额外信息，空格分隔，属性值由双引号括起来。

## URL 简介

构成：协议、域名、端口号、路径、查询参数  
合法字符规定：字母、数字、下划线、点、中连线，其余字符均需要经过转义。

## HTML 全局属性

- [id](https://www.bookstack.cn/read/html-tutorial/spilt.1.spilt.2.docs-attribute.md)
- [class](https://www.bookstack.cn/read/html-tutorial/spilt.2.spilt.2.docs-attribute.md)
- [title](https://www.bookstack.cn/read/html-tutorial/spilt.3.spilt.2.docs-attribute.md) 为元素添加附加说明。
- [tabindex](https://www.bookstack.cn/read/html-tutorial/spilt.4.spilt.2.docs-attribute.md)Tab 键，遍历网页元素。
- [accesskey](https://www.bookstack.cn/read/html-tutorial/spilt.5.spilt.2.docs-attribute.md) 指定网页元素获得焦点的快捷键。
- [style](https://www.bookstack.cn/read/html-tutorial/spilt.6.spilt.2.docs-attribute.md)
- [hidden](https://www.bookstack.cn/read/html-tutorial/spilt.7.spilt.2.docs-attribute.md) 布尔属性，优先级低于 CSS 的可见性设置。
- [lang，dir](https://www.bookstack.cn/read/html-tutorial/spilt.8.spilt.2.docs-attribute.md) 语言及文字阅读方向。
- [contenteditable](https://www.bookstack.cn/read/html-tutorial/spilt.9.spilt.2.docs-attribute.md)
- [spellcheck](https://www.bookstack.cn/read/html-tutorial/spilt.10.spilt.2.docs-attribute.md)
- [data-属性](https://www.bookstack.cn/read/html-tutorial/spilt.11.spilt.2.docs-attribute.md) 放置自定义数据。
- [事件处理属性](https://www.bookstack.cn/read/html-tutorial/spilt.12.spilt.2.docs-attribute.md) 事件处理属性。

## Script 标签

### defer vs async 属性

![[Pasted image 20220704182525.png]]

### onload 事件

`src` 指定资源加载执行完毕后调用

## HTML 标签

### meta

例如：

```html
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width,initial-scale=1">
	<meta name="description" content="HTML <meta> 元素表示那些不能由其他 HTML 元相关（meta-related）元素表示的元数据信息。如：<base>、<link>、<script>、<style> 或 <title>。">
</head>
```

查找：

```js
export function getMetaContentByName(name) {
	const meta = document.head.querySelector(`[name=${name}]`)
	if(meta) return meta.content
	return ''
}
```

元数据（MetaData），不会显示在页面，但是会被浏览器解析，常用于指定网页描述、关键词、作者、最后更新时间等。  
[<meta>：元数据元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)

## 事件模型/事件机制/事件流

### 什么是事件流

`DOM` 树中的元素触发事件时，该事件就会在树的根节点与元素节点之间传递，经过的元素也会收到该事件，这种事件传递过程就叫**事件流**

#### 三阶段

- 捕获
- 目标
- 冒泡

#### 3 种方式绑定事件

- `DOM0` 级（`on + type`），只支持事件冒泡，缺点是每个元素只能被绑定一个回调；
- `DOM2` 级（`addEventListener`），`IE6-8` 不兼容这个方法，在低版本的 `IE` 中需要使用 `attachEvent`；
- 通过 `HTML` 属性，不建议，缺点视图与逻辑耦合在一起，减少可读性与可维护性，同时 `HTML` 文件变大；

#### 事件委托

事件代理是**利用事件冒泡机制**处理指定一个事件处理程序，来管理某一类型的所有事件，将同类元素的事件委托给父级或者更外级的元素，使用事件代理/委托后：

- 不必为每个列表元素绑定事件，消耗内存；
- 不必为动态添加/删除的元素重新绑定事件；
- `event.target == event.currentTarget`，让触发事件的元素等于绑定事件的元素；

```ad-info
`event.target` 与 `event.currentTarget` 区别

- `event.target` 返回**触发事件**的元素
- `event.currentTarget` 返回**绑定事件**的元素，只有被点击的那个目标元素的 `event.target`才会等于 `event.currentTarget`
```

#### 一些方法

`event.stopPropagation()`：阻止事件冒泡；  
`event.preventDefault()`：阻止事件默认行为；

#### Vue 中的事件修饰符

- `.stop` 阻止事件冒泡
- `.prevent` 阻止默认行为
- `.once` 只触发一次
- `.capture` 捕获阶段执行
- `.passive`：告诉浏览器不要阻止默认行为
- `.self` 只在元素本身触发

# Reference

[async vs defer attributes](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)
