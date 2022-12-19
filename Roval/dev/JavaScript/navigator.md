Created Date：2022-12-17 22:04:47  
Last Modified：2022-12-17 22:04:46

# Tags

#JavaScript

# Content

		
		- 通常用于确定浏览器的类型
				
		- userAgent：返回浏览器的用户代理字符串
				

const ua = navigator.userAgent;  
export default {  
 isMobile: /Android|webOS|iPhone|ipad|iPod|iOS|BlackBerry|Windows Phone|Tablet/i.test(ua),  
 isBiliApp: (/BiliApp/i).test(ua),  
 isIOS: /iPhone|ipad|iPod|iOS/i.test(ua),  
 isWindows: /windows/i.test(ua),  
 isMacOS: /macintosh/i.test(ua),  
}

# Reference
