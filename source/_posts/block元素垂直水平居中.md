---
title: block元素垂直水平居中
categories: note
tag: [HTML, CSS]
date: 2017-08-06 18:38:53
updated: 2017-08-06 18:38:53
---
### case 1 使用绝对定位

#### 场景 1-1
	
* 父元素宽高固定；
* 子元素宽高固定；

**HTML**

```html
<div class="wrap">
  <div class="center"></div>
</div>
```
**CSS**

```css
.wrap {
    position: relative;
    width: 700px;
    height: 500px;
    background-color: gray;
}
.center {
    position: absolute;
    width: 200px;
    height: 200px;
    left: 250px;
    top: 150px;
    background-color: black;
}
```

**Tip**

* left：父元素宽度700px ／ 2 - 子元素宽度200px ／ 2 = 250px
* top: 父元素高度500px / 2 - 子元素高度200px / 2 = 150px

[Demo](https://codepen.io/zhangxiaoshang/pen/yogMqr)


#### 场景 1-2 使用calc

* 父元素或子元素的宽、高是相对值，无法确定具体大小；
* 亦适用于场景 1-1

**HTML**

```html
<div class="wrap">
  <div class="center"></div>
</div>
```
**CSS**

```css
.wrap {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-color: gray;
}
.center {
    position: absolute;
    width: 20%;
    height: 20%;
    left: calc(100vw / 2 - 20% / 2);
    top: calc(100vh / 2 - 20% / 2);
    background-color: black;
}
```

**Tip**

* `vw`/`vh`分别表示相对视口的宽度和高度， 100vw表示100%的视口宽度；
* `calc()`是`CSS`的一个函数，可以通过计算决定一个属性的值；

[Demo](https://codepen.io/zhangxiaoshang/pen/ZJLKvp)

#### 场景 1-3 使用translate

* 适用场景1-1、1-2；

**HTML**

```html
<div class="wrap">
  <div class="center"></div>
</div>
```
**CSS**

```css
.wrap {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-color: gray;
}
.center {
    position: absolute;
    width: 20%;
    height: 20%;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background-color: black;
}
```

**Tip**

* 设置`left: 50%`、`top: 50%`后，黑色方块的左上角在父元素中心位置；
* `transform: translate(-50%, -50%)`使黑色方块分别向左、向上移动相对于自身宽、高50%的距离；

[Demo](https://codepen.io/zhangxiaoshang/pen/GvrmeL)

#### 场景 1-4 绝对定位联合margin

* 适用场景1-1、1-2；

**HTML**

```html
<div class="wrap">
  <div class="center"></div>
</div>
```
**CSS**

```css
.wrap {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-color: gray;
}
.center {
    position: absolute;
    width: 20%;
    height: 20%;
    background-color: black;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: auto;
}
```

**Tip**

* 不关心元素尺寸，不需要计算

[Demo](https://codepen.io/zhangxiaoshang/pen/OjWgPJ)

### case 2 flex布局

#### 场景 2-1

* 不关心元素尺寸

**HTML**

```html
<div class="wrap">
  <div class="center"></div>
</div>
```
**CSS**

```css
.wrap {
    display: flex;
    width: 100vw;
    height: 100vh;
    background-color: gray;
}
.center {
    width: 20%;
    height: 20%;
    background-color: black;
    margin: auto;
}
```

**Tip**

* 父元素使用`flex`布局
* 子元素设置`margin: auto`

[Demo](https://codepen.io/zhangxiaoshang/pen/OjWgPJ)

#### 场景 2-2

* 不关心元素尺寸
* 不需要设置子元素

**HTML**

```html
<div class="wrap">
  <div class="center"></div>
</div>
```
**CSS**

```css
.wrap {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-color: gray;
}
.center {
    width: 20%;
    height: 20%;
    background-color: black;
}
```

**Tip**

* 父元素使用`flex`布局
* `justify-content: center`：父元素内容水平居中
* `align-items: center`：父元素内容垂直居中

[Demo](https://codepen.io/zhangxiaoshang/pen/WEROxG)

