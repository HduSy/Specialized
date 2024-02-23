Created Date：2024-02-23 14:35:22  
Last Modified：2024-02-23 14:35:22

# Tags

# Content

```js
// 1.设置v-throttle自定义指令
Vue.directive('throttle', {
  bind: (el, binding) => {
    let throttleTime = binding.value; // 节流时间,该时间内只能触发一次
    if (!throttleTime) { // 用户若不设置节流时间，则默认2s
      throttleTime = 2000;
    }
    let cbFun; // 节流标识
    el.addEventListener('click', event => {
      if (!cbFun) { // 第一次执行
        cbFun = setTimeout(() => {
          cbFun = null;
        }, throttleTime);
      } else {
        event && event.stopImmediatePropagation();
      }
    }, true);
  },
});
// 2.为button标签设置v-throttle自定义指令
<button @click="sayHello" v-throttle>提交</button>
```

# Reference

[Vue 指令-应用场景 | 前端那些事儿](https://jonny-wei.github.io/blog/vue/vue/vue-directive.html#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)
