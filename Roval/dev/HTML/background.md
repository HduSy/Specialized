# Background

- `background-color` 属性值为颜色值或 `transparent` 关键字
		
- `background-attachment` 指定图片是否跟随内容滚动
		

		值

		

		描述

		

		scroll

		

		默认值，随页面滚动而滚动

		

		fixed

		

		不随页面滚动而滚动

		

		local

		

		随元素内容滚动

		

		initial

		

		设置默认值

		

		inherit

		

		继承属性值

		
- `background-origin` 设置图片定位原点，当 background-attachment 是 fixed 时，该属性失效
		

		值

		

		描述

		

		padding-box

		

		padding区域，默认值

		

		border-box

		

		border区域

		

		content-box

		

		content区域

		
- `background-position` 图像起始位置
		

		值

		

		描述

		

		left top left center left bottom right top right center right bottom center top center center center bottom

		

		如果只设置一个值，另一个值为center

		

		_x% y%_

		

		第一个值水平方向，第二个值垂直方向，如果只指定一个值，另一个是50%；另外左上角为`0% 0%`，右下角为`100% 100%`；默认为左上角

		

		_xpos ypos_

		

		第一个值水平方向，第二个值垂直方向，如果只指定一个值，另一个是50%；<length>+任意CSS单位，左上角为`0 0`

		

		inherit

		

		继承

		
- `background-size` 设置背景图大小
		

		值

		

		描述

		

		auto

		

		默认值

		

		length

		

		第一个值指定宽度，第二个值指定高度，若未指定为`auto`

		

		%

		

		相对于容器百分比，第一个值指定宽度，第二个值指定高度，若未指定为`auto`

		

		contian

		

		等比缩放直至容器完整显示背景图

		

		cover

		

		等比缩放直至背景图覆盖容器

		
- `background-repeat` 设置背景图重复方式
		

		值

		

		描述

		

		repeat

		

		水平&垂直方向上重复

		

		repeat-x

		

		水平方向上

		

		repeat-y

		

		垂直方向上

		

		no-repeat

		

		不重复

		

		inherit

		

		继承

		
- `background-clip` 背景覆盖方式
		

		值

		

		描述

		

		border-box

		

		border区域及以内，默认值

		

		padding-box

		

		padding区域及以内

		

		content-box

		

		content区域及以内

		
- `background-image`
		

		值

		

		描述

		

		url(_'URL'_)

		

		指定图片url

		

		[linear-gradient()](https://www.runoob.com/cssref/func-linear-gradient.html)

		

		线性渐变

		

		[radial-gradient()](https://www.runoob.com/cssref/func-radial-gradient.html)

		

		径向渐变

		

		[repeating-linear-gradient()](https://www.runoob.com/cssref/func-repeating-linear-gradient.html)

		

		重复线性渐变

		

		[repeating-radial-gradient()](https://www.runoob.com/cssref/func-repeating-radial-gradient.html)

		

		重复径向渐变

		

		inherit

		

		继承

		

		none

		

		未指定图像时显示，可为颜色
