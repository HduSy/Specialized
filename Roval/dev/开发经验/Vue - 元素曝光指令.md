Created Date：2023-11-23 16:53:40  
Last Modified：2023-11-23 16:53:39

# Tags

#Vue #前端埋点

# Content

## 摘要

埋点按照获取数据的方式一般可以分为以下 3 种：

- 页面埋点：统计用户进入或离开页面的各种维度信息，如页面浏览次数（`PV`）、浏览页面人数（`UV`）、页面停留时间、浏览器信息等（可自动上报）；
- 点击埋点：统计用户在应用内的每一次点击事件（可自动上报）；
- 曝光埋点：统计具体区域是否被用户浏览到（自动上报不能满足相应的需求，所以需要手动调用接口方式进行埋点数据上报）；

### 有效曝光逻辑

曝光埋点由于涉及到**有效曝光逻辑的判断**，例如：

- 商品卡片必须完全的出现在浏览器可视化区域内；
- 商品必须在可视化区域内停留 5s 以上；
- 用户进入页面到离开页面相同的商品只进行一次曝光；
- …等其他自定义业务逻辑

## 实践

### IntersectionObserver

#### 兼容性

![[Pasted image 20240108105223.png]]

#### API

```js
let options = {
    root: document.querySelector('#scrollArea'),
    rootMargin: '0px',
    threshold: 1.0
}
let callback =(entries, observer) => {
  entries.forEach(entry => {});
};
let observer = new IntersectionObserver(callback, options);
```

接受两个参数：`callback` 是可见性变化时的回调函数，`option` 是配置对象（该参数可选），返回一个 `observer` 实例  

##### `option`

- root：指定根 (**root**) 元素，用于检查目标（**target**）的可见性。必须是目标元素的父级元素，如果未指定或者为 `null`，则默认为浏览器视窗；
- rootMargin：根元素的 `margin`，类似于 `CSS` 中的 `margin` 属性，默认值为 `0`；
- threshold：`target` 和 `root` 相交程度达到该值时 `callback` 函数将会被执行，可以是单一的 `Number` 也可以是 `Number` 数组，当为数组时每达到该值都会执行 `callback` 函数；  

##### `callback(entries: IntersectionObserverEntry[], observer: IntersectionObserver)`

- 参数 `entries` 是一个数组，每个成员都是一个 `IntersectionObserverEntry` 对象；
- 参数 `observer` 是被调用的 `IO` 实例；  

`callback` 函数一般会被调用两次，一次是目标元素进入可视化区域，另一次是离开可视化区域，配置 `options.threshold` 会影响 `callback` 函数的调用次数

##### `IntersectionObserverEntry`

- `IntersectionObserverEntry.target`：需要观察的目标元素，是一个 DOM 节点对象；
- `IntersectionObserverEntry.boundingClientRect`：返回**目标元素的边界信息**，边界的计算方式与 `Element.getBoundingClientRect()` 相同；
- `IntersectionObserverEntry.intersectionRect` ：用来描述根和目标元素的**相交区域的信息**；
- `IntersectionObserverEntry.intersectionRatio`：返回 `intersectionRect` 与 `boundingClientRect` 的比例值，**0 为完全不可见，1 为完全可见**；
- `IntersectionObserverEntry.isIntersecting`：返回一个布尔值, 如果根与目标元素相交（即从不可视状态变为可视状态），则返回 `true`；如果返回 `false`, 变换是从可视状态到不可视状态；
- `IntersectionObserverEntry.rootBounds` ：**根元素的区域信息**；
- `IntersectionObserverEntry.time`：可见性状态发生改变时间的**时间戳**，单位为毫秒；

##### `IO Instance Method`

- `IntersectionObserver.observe()`： 使 `IO` 开始监听特定目标元素；
- `IntersectionObserver.unobserve()`：使 `IO` 停止监听特定目标元素；
- `IntersectionObserver.disconnect()`：使 `IO` 对象停止监听工作；
- `IntersectionObserver.takeRecords()`：返回所有观察目标的 `IO` 对象数组；  

### getBoundingClientRect

![[Pasted image 20240108105208.png]]

### 代码

#### 业务中自实现

曝光检测兼容性代码：

```js
const exposure =  (() => {
  if (typeof window === 'undefined') {
    return () => {}
  }
  if ('IntersectionObserver' in window) {
    const observer = new IntersectionObserver((entries, observer) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const el = entry.target
          el.__exposure__?.handler?.()
          observer.unobserve(el)
        }
      })
    }, {
      threshold: 0.5
    })
    return (el, handler) => {
      el.__exposure__ = { ...el.__exposure__, handler }
      observer.observe(el)
      return () => observer.unobserve(el)
    }
  } else {
    const checkInView = (dom, preload = 0) => {
      if (!dom) {
        return false
      }
      const rect = dom.getBoundingClientRect()
      return (
        rect.top < window.innerHeight + preload &&
        rect.bottom + preload > 0 &&
        rect.left < window.innerWidth + preload &&
        rect.right + preload > 0
      )
    }
    const getScrollParent = dom => {
      let el = dom
      if (!el) {
        return null
      }
      while (el && el.tagName !== 'HTML' && el.tagName !== 'BODY' && el.nodeType === 1) {
        const overflowY = window.getComputedStyle(el).overflowY
        if (overflowY === 'scroll' || overflowY === 'auto') {
          if (el.tagName === 'HTML' || el.tagName === 'BODY') {
            return document
          }
          return el
        }
        el = el.parentNode
      }
      return document
    }

    return (el, handler) => {
      if (checkInView(el)) {
        handler()
        return () => {}
      } else {
        const parent = getScrollParent(el)
        const handleChange = () => {
          if (checkInView(el)) {
            handler()
            parent.removeEventListener('resize', handleChange)
            parent.removeEventListener('scroll', handleChange)
          }
        }
        parent.addEventListener('resize', handleChange, { passive: true })
        parent.addEventListener('scroll', handleChange, { passive: true })
        return () => {
          parent.removeEventListener('resize', handleChange)
          parent.removeEventListener('scroll', handleChange)
        }
      }
    }
  }
})()
export default exposure

```

指令注册：

```js
Vue.directive('exposure', {
	// 指令绑定时执行，传递指令值即曝光函数
    bind(el, binding) {
      el.__exposure__ = { handler: binding.value } // 指令邦定值
    },
    // 被绑元素插入父节点时调用，执行曝光函数对元素进行曝光检测并返回取消曝光检测方法
    inserted(el) {
      el.__exposure__.cancel = exposure(el, el.__exposure__.handler)
    },
    // 指令解绑时调用，取消曝光监听
    unbind(el) {
      el.__exposure__?.cancel?.()
    },
  })
```

使用：

```vue
<div v-exposure="() => loggerReport('key', 'value')"></div>
```

#### 政采云团队实现

`Vue` 指令

```js
const options = {
    root: null, //默认浏览器视窗
    threshold: 1 //元素完全出现在浏览器视窗内才执行callback函数。
}
const callback =(entries, observer) => {
  entries.forEach(entry => {});
};
const observer = new IntersectionObserver(callback, options);
const addListenner = (ele, binding) => {
    observer.observe(ele);
};
const removeListener = (ele) => {
  observer.unobserve(ele);
};
//自定义曝光指令
Vue.directive('visually', {
  bind: addListenner,
  unbind: removeListener,
});
```

不重复上报通过维护一个全局数组 `visuallyList` 实现：

```js
let visuallyList = []; //记录已经上报过的埋点信息
const addListenner = (ele, binding) => {
    if(visuallyList.indexOf(binding.value) !== -1) return;
    
    observer.observe(ele);
};
```

曝光超过 5s 才上报通过 `setTimeout` 实现：

```js
let timer = {}; //增加定时器对象
const callback = entries => {
  entries.forEach(entry => {
    let visuallyData = null;
    try {
      visuallyData = JSON.parse(entry.target.getAttribute('visually-data'));
    } catch (e) {
      visuallyData = null;
      console.error('埋点数据格式异常', e);
    }
    //没有埋点数据取消上报
    if (!visuallyData) {
      observer.unobserve(entry.target);
      return;
    }
    
    if (entry.isIntersecting) {
      timer[visuallyData.id] = setTimeout(function() {
        //上报埋点信息
        sendUtm(visuallyData).then(res => {
          if (res.success) {
            //上报成功后取消监听
            observer.unobserve(entry.target);
            visuallyList.push(visuallyData.id);
            timer[visuallyData.id] = null;
          }
        });
      }, 5000);
  } else {
    if (timer[visuallyData.id]) {
      clearTimeout(timer[visuallyData.id]);
      timer[visuallyData.id] = null;
    }
  }
  });
};
```

# Reference

[通过自定义Vue指令实现前端曝光埋点 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000039746521)  
[[../杂/技术博客与网站#阮一峰|技术博客与网站]]
