Created Date：2023-11-23 16:53:40  
Last Modified：2023-11-23 16:53:39

# Tags

# Content

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

[[../杂/MDN|MDN]]
