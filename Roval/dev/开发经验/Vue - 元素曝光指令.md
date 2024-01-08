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

# Reference

[通过自定义Vue指令实现前端曝光埋点 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000039746521)  
[[../杂/技术博客与网站#阮一峰|技术博客与网站]]
