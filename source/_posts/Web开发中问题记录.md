---
title: Angularjs src加载图片404
categories: log
tags: AngularJS
date: 2017-08-24 22:46:44
updated: 2017-08-24 22:46:44
---
### Angularjs src与ng-src
**问题：** 在Angularjs(v1.x)项目中，`img`标签中使用`src = { { url } }`这种形式的数据绑定，在页面模版初次加载时，会有一个`404的`请求错误，原因在于，在Angularjs编译模版前，浏览器会把`{ { url } }`当作普通字符串，向`{ { url } }`发出请求，这个地址自然是不存在的，从而导致`404`.

**解决：** 使用`ng-src = { { url } }`代替`src = { { url } }`, 这样在模版初次加载时就不会发出错误的请求了！

*Tip:* 如果`src`的值时字符串常量，则无论使用`src`还是`ng-src`自然都不会有问题！