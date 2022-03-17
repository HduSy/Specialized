### 规则
-   \s 空白字符串
-   \w 字母、数字、下划线。A~Z、a~z、0~9、_中任意一个
- ![[Pasted image 20220307165426.png]]
字符集中的`.和*`不必进行转义。
- ![[Pasted image 20220307165656.png]]

### 实践
```js
const str = 'data:image/png;base64,iVBORw0KG...' // base64格式
const regx = /^data:image\/(\w+)/ // 文件类型
const res = str.match(regx) // ['data:image/png', 'png']
const ext = res[1]
```