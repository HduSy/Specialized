1.  location对象
    
    -   window.location与document.location指向同一对象
        
    -   查询字符串location.search返回URL中?及之后的字符串通过手段可解析URL参数为`key-value`对象；URLSearchParams提供一套API直接操作查询字符串
        
    -   操作地址有三种方式，location.href = ；window.location = ；location.assign()；其中第一种最常用。
        
    -   修改location的属性都会导致重新加载，属性列表如下：
        
    -   replace方法不会生成新的历史记录，浏览器`后退`按钮失效不可点击
        
2.  navigator对象
    
    -   通常用于确定浏览器的类型
        
    -   userAgent：返回浏览器的用户代理字符串
        

const ua = navigator.userAgent;  
export default {  
 isMobile: /Android|webOS|iPhone|ipad|iPod|iOS|BlackBerry|Windows Phone|Tablet/i.test(ua),  
 isBiliApp: (/BiliApp/i).test(ua),  
 isIOS: /iPhone|ipad|iPod|iOS/i.test(ua),  
 isWindows: /windows/i.test(ua),  
 isMacOS: /macintosh/i.test(ua),  
}

3.  window对象
    
    -   作用域：BOM中window===JS中global，JS中通过let\const定义的变量却不会成为window属性
        
    -   窗口关系：`.top`:最上层/外层对象窗口；`.parent`:父窗口；`.self`:始终指向window对象
        
    -   窗口位置：`.screenLeft`、`.screenTop`：窗口距离屏幕左侧/上侧的CSS像素；`.moveTo(x,y)`、`.moveBy(xL,yL)`：移动窗口;
        
    -   窗口大小： `innerWidth、innerHeight、outterWidth、outterHeight`：页面视口大小/浏览器窗口自身大小；`resizeTo(w,h)、resizeBy(wL,hL)`：缩放窗口大小；
        
    -   视口位置：浏览器窗口大小通常无法完整显示整个页面，度量文档相对于视口滚动距离的属性有`window.pageXOffset\window.scrollX、window.pageYOffset\window.scrollY`；`scroll()\scrollTo(x,y)\scrollBy(xL,yL)`
        
    -   导航与打开：`window.open(url,targetWin,options,replace:boolean),targetWin also can be _self、_parent、_top or _blank`、`window.close()`