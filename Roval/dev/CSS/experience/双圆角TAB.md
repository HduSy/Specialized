Created Date：2024-01-12 14:42:46  
Last Modified：2024-01-12 14:42:46

# Tags

[[../css|css]]

# Content

## Show Me The Code

```scss
// 激活时
.active {
  background-color: #FFF;
  border-radius: 12px 12px 0 0;
  box-shadow: 12px 12px 0 0 #FFF, -12px 12px 0 0 #FFF; // 覆盖外圆角未覆盖的区域,阴影色与激活态Tab颜色相同
  z-index: 1;
  // 前后添加为元素覆盖
  &::before {
    content: '';
    position: absolute;
    bottom: 0;
    width: 12px;
    height: 100%;
    left: -12px;
    border-radius: 0 0 12px 0; // 重要
    background-color: #F6F7F8; // 上色
  }
  &::after {
    content: '';
    position: absolute;
    width: 12px;
    height: 100%;
    right: -12px;
    bottom: 0;
    border-radius: 0 0 0 12px; // 重要
    background-color: #F6F7F8; // 上色，与未激活Tab颜色相同
  }
}
```

# Reference

[CSS3 实现双圆角 Tab 菜单 - 掘金](https://juejin.cn/post/7070906612885487624#heading-3)  
[实现tabs圆角及反圆角效果 - 掘金](https://juejin.cn/post/7224311569777934392)
