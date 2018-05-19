---
title: 《响应式Web设计HTML5和CSS3实战（第2版）》读书笔记
categories: note
date: 2018-05-19 19:35:49
updated: 2018-05-19 19:35:49
tags: HTML
toc: true
---

## 章节
### 一、响应式Web设计基础
### 二、媒体查询
### 三、弹性布局与响应式图片
### 四、HTML5与响应式Web设计
* HTML5新语义元素
    * main
        * 指文章中特有的内容，导航链接、版权信息、站点标志、广告和搜索表单等多个文档中重复出现等内容不算主内容
    * section
        * 用于定义文档或应用中一个通用的区块。例如，可以用来包装联系信息、新闻源等。这个元素不是为应用样式而存在的。如果只是为了添加样式而包装内容，还是使用div。
    * nav
        * ul > li 写的导航最好改成 nav > a
    * acticle
        * 用于包含一个独立的内容块。在划分页面结构时，问一问自己，想放在article中的内容如果整体复制到另一个站点中是否照样有意义？明显可以放到article元素中的内容有博客正文和新闻报道。
    * aside
        * 用于包含与其旁边内容不相关的内容。这个元素也适合包装突出引用、广告和导航元素。基本上任何与主内容无直接关系的，都可以放在这里边。
    * figure & figcaption
        * 用于包含注解、图示、照片、代码，等等。如果图片或代码需要一个小标题，那么这个元素非常合适。
    * detail & summary
        * 可以实现一个 展开/收起 部件
    * header
        * 用在站点页头用作“报头”，或者在<article>元素中用作某个区块的引介区。
    * footer
        * 在用于在相应区块中包含与区块相关的内容，可以包含指向其他文档的链接，或者版权声明。比如可以用它作为博客的页脚，同时用它包含文章正文的末尾部分。不过，规范里说了，作者的联系信息应该放在<address>元素中。
    * address
        * 明显用于标记联系人信息，为最接近的<article>或<body>所用。不过有一点不好理解，它并不是为包含邮政地址准备的（除非该地址确实是相关内容的联系地址）。邮政地址以及其他联系人信息应该放在传统的<p>标签里。
    * h1 到 h6
        * 规范是不推荐使用h1到h6来标记标题和副标题的。比如这样：h1+h2，根据规范的建议应该写成这样h1+p （+表示兄弟元素）
* HTML5文本级元素
    * b
        * 只为引人注意而标记的文本，不传达更多的重要性信息，也不用于表达其他的愿望或情绪。它是文本级的，不能用来包围其他标记，这时候应该用div。
    * em
        * 表示内容中需要强调的部分
    * i
        * 一段文本用于表示另一种愿望或情绪，或者以突出不同文本形式的方式表达偏离正文的意思

### 五、CSS3新特性
### 六、CSS3高级技术
### 七、SVG与响应式Web设计
### 八、CSS3过渡、变形和动画
### 九、表单
### 十、实现响应式Web设计
## 资料
### CSS长度单位 https://benfrain.com/just-use-pixels/
### 行内块与空白 https://css-tricks.com/fighting-the-space-between-inline-block-elements/
### 渐进增强例子 http://www.cssmojo.com/how-to-style-a-carousel/
### 渐进增强例子 http://www.cssmojo.com/use-radio-buttons-for-single-option/
## 工具
### Autoprefixer https://github.com/postcss/autoprefixer
### ico icomoon.io
### 矩阵变形工具 http://www.useragentman.com/matrix/
### 正则 https://regexr.com/
### 浏览器窗口同步 https://browsersync.io/
### 页面性能可视化 http://www.webpagetest.org/