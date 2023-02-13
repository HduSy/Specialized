Created Date：2023-02-10 15:09:38  
Last Modified：2023-02-10 15:09:37

# Tags

# Content

## match

![[Pasted image 20230213114327.png]]

### 实践

```js
const str = 'ComponentNameTestRegexp'
console.log(stra.match(/([a-z\d])([A-Z])/g))
// (3)['tN', 'eT', 'tR']
console.log(stra.match(/([a-z\d])([A-Z])/))
// (3)['tN', 't', 'N', index: 8, input: 'ComponentNameTestRegexp', groups: undefined]
```

## replace

![[Pasted image 20230213145649.png]]

# Reference
