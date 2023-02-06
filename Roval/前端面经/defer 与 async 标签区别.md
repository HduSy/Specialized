Created Date：2023-02-06 22:15:29  
Last Modified：2023-02-06 22:15:29

# Tags

#面经

# Content

## \<script\>

脚本的下载与执行会打断 HTML 解析过程，在 [[DOMContentLoaded]] 之前执行完所有脚本。

## \<script async\>

脚本的下载与 HTML 解析过程同时进行，但脚本下载完成后会 **立即执行**，打断 HTML 解析过程。

## \<script defer\>

脚本的下载过程同上，但执行会在 HTML 解析完成之后执行，且按脚本书写顺序执行。

# Reference

[async vs defer attributes - Growing with the Web](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)
