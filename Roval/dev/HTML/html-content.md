### 双向链接
摘要：记录一些HTML学习小点，因为本身HTML元素及属性就多而无逻辑可言，且更新较快，不可能记着所有；另外一些实践中遇到的问题对应解决方案对于文档本身理解也是极其珍贵的。

[[background]]
### HTML简介
概述：HTML是网页的语言，描述网页内容与结构，从服务器下载HTML文件，浏览器将网页渲染出来。
基本概念：网页由各种各样的**标签**构成，浏览器会渲染标签内容展示出来。浏览器在渲染网页的时候，会把HTML源码解析成一个标签树，每个标签都是一个节点，称为**元素**（Element），标签是针对于HTML结构的叫法，节点是针对于编程上的叫法，两者是同义词。**属性**是标签的额外信息，空格分隔，属性值由双引号括起来。
### URL简介
构成：协议、域名、端口号、路径、查询参数
合法字符规定：字母、数字、下划线、点、中连线，其余字符均需要经过转义。
### HTML全局属性
-   [id](https://www.bookstack.cn/read/html-tutorial/spilt.1.spilt.2.docs-attribute.md)
-   [class](https://www.bookstack.cn/read/html-tutorial/spilt.2.spilt.2.docs-attribute.md)
-   [title](https://www.bookstack.cn/read/html-tutorial/spilt.3.spilt.2.docs-attribute.md)为元素添加附加说明。
-   [tabindex](https://www.bookstack.cn/read/html-tutorial/spilt.4.spilt.2.docs-attribute.md)Tab 键，遍历网页元素。
-   [accesskey](https://www.bookstack.cn/read/html-tutorial/spilt.5.spilt.2.docs-attribute.md)指定网页元素获得焦点的快捷键。
-   [style](https://www.bookstack.cn/read/html-tutorial/spilt.6.spilt.2.docs-attribute.md)
-   [hidden](https://www.bookstack.cn/read/html-tutorial/spilt.7.spilt.2.docs-attribute.md)布尔属性，优先级低于CSS 的可见性设置。
-   [lang，dir](https://www.bookstack.cn/read/html-tutorial/spilt.8.spilt.2.docs-attribute.md)语言及文字阅读方向。
-   [contenteditable](https://www.bookstack.cn/read/html-tutorial/spilt.9.spilt.2.docs-attribute.md)
-   [spellcheck](https://www.bookstack.cn/read/html-tutorial/spilt.10.spilt.2.docs-attribute.md)
-   [data-属性](https://www.bookstack.cn/read/html-tutorial/spilt.11.spilt.2.docs-attribute.md)放置自定义数据。
-   [事件处理属性](https://www.bookstack.cn/read/html-tutorial/spilt.12.spilt.2.docs-attribute.md)事件处理属性。