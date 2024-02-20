Created Date：2022-12-17 22:06:10  
Last Modified：2022-12-17 22:06:09

# Tags

#JavaScript

# Content

- \s 空白字符串
- \w 字母、数字、下划线。A~Z、a~z、0~9、_ 中任意一个
- ![[Pasted image 20220307165426.png]]  
字符集中的 `.和*` 不必进行转义。
- ![[Pasted image 20220307165656.png]]

## 实践

```js
const str = 'data:image/png;base64,iVBORw0KG...' // base64格式
const regx = /^data:image\/(\w+)/ // 文件类型
const res = str.match(regx) // ['data:image/png', 'png']
const ext = res[1]
```

## 在 String 方法中的应用

### [[String#match]]

### [[String#replace]]

```js
const reg = /(\d+(\.\d{1,})?)(\D*)/; // $1-$n：小括号里的正则匹配结果
// $1: 数字部分 $2：小数点部分 $3：后缀部分
export const getNumber = (attrValue: string | number) => {
  return typeof attrValue === 'string'
    ? Number(attrValue.replace(reg, '$1')) || 0
    : attrValue;
};

const width = '12px'
width.replace(reg, '$1') // 12
width.replace(reg, '$1$3') // 12px
因为没有小数点部分所以，$2为空字符串
```

## 正则匹配 URL

[正则匹配url path-掘金](https://juejin.cn/s/%E6%AD%A3%E5%88%99%E5%8C%B9%E9%85%8Durl%20path)

# Reference
