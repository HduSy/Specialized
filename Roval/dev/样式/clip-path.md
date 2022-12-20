Created Date：2022-12-19 00:24:35  
Last Modified：2022-12-20 23:58:31

# Tags

 #CSS

# Content

裁切元素形状。

```ad-tip
我在 CodePen 上的实践：[clip-path](https://codepen.io/hdusy/pen/QWBWavY)
```

## none

## shape-box

`margin-box`、`border-box`、`padding-box`、`content-box`

## basic-func

内置函数。

### inset

内部裁切，由外向内裁，参数：上、右、下、左要裁切的距离或上下、左右。

```css
clip-path: inset(100px 50px);
/* 相当于 */
clip-path: inset(100px 50px 50px 100px);
```

### circle

圆形裁切。参数：`半径` at `圆心x轴坐标` `圆心y轴坐标`

```css
clip-path: circle(100px at 50% 50%);
```

### ellipse

椭圆形裁切。参数：`x轴半径 y轴半径` at `椭圆心x轴坐标 椭圆心y轴坐标`

```css
clip-path: ellipse(100px 50px at 50% 50%);
```

### polygon

不规则形状裁切，参数：顺时针坐标点指定。

```css
clip-path: polygon(50px 0, 350px 0, 300px 400px, 0 400px);
```

## image-shape

url 链接图片、linear-gradient 等。

# Reference

1. [Clippy — CSS clip-path maker](https://bennettfeely.com/clippy/)  
2. 一个将 clip-path 用到极致的神奇网站 [In Pieces - 30 Endangered Species, 30 Pieces.](http://species-in-pieces.com/)
