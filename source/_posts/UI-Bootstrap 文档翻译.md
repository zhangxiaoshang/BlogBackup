---
title: UI-Bootstrap文档翻译
categories: note
tags: AngularJS
date: 2017-03-19 20:44:52
---
### UI Bootstrap

AngularUI 团队使用纯AngularJS写的Bootstrap 组件


### Angular 2

为Angular 2 提供支持，检验ng-bootstrap, 由 UI Bootstrap 团队创建。

### 依赖

这个指令库包含了符合Bootstrap标准和样式的一组原生的AngularJS指令。而且不依赖于JQuery或者Bootstrap的javascript文件。它只依赖这些：

* AngularJS(要求AngularJS 1.4.x或者更高版本，已在1.6.1版本测试). 0.14.3版本的库是支持AngularJS 1.3.x 的最新版本, 0.12.0版，是支持AngularJS 1.2.x的最新版本
* Angular-animate (这个版本需要和你的angular匹配，1.6.1版本已测试)，如果你打算使用动画，你需要加载angular-animate
* angular-touch (这个版本需要和你的angular匹配，1.6.1版本已测试), 如果你打算使用swipe actions， 你需要加载angular-touch.
* Bootstrap CSS (3.3.7版本已测试). 这个版本的库（2.5.0）只能与Bootstrap CSS 3.x版本使用。0.8.0版本的库是支持Bootstrap CSS 2.3.x的最新版本

### 文件下载

所有指令的构建文件分布几个不同的种类：为产品使用的压缩文件，为开发使用的非压缩文件，有或没有模板。所有的配置和描述说明可以[从这里下载](https://github.com/angular-ui/bootstrap/tree/gh-pages), 有`-tpls`标识的文件包含捆绑了JavaScript的模板，正常的版本不包含被捆绑的模板，如果你只对这些指令中的一部分感兴趣，你可以在[这里创建你需要的文件然后下载](http://angular-ui.github.io/bootstrap/)(需要在原文创建)。

无论你选择用哪种方式，好消息是，所有这些文件的大小相当小：包含模板的压缩文件只有122k, 没有模板的是98k（如果使用gzip压缩，有模板的是 ~31kb ， 没有模板是28k）。

### 安装

当你下载了所有的文件，并在你的页面中引用它们，你只需要在模块中声明依赖`ui.bootstrap`：

	angular.module('myModule', ['ui.bootstrap']);
	
如果你在CSP模块中使用UI Bootstrap， 例如：在扩展中，确保手动在HTML文件中链接`ui-bootstrap-csp.css` 

你可以从这个页面去plunkers查看对应描述的demo的运行示例。

### 前缀迁移

自从0.14.0版本，我们开始给所有的组件增加前缀。如果你是从ui-bootstrap 0.13.4或更早的版本升级，请查看我们的[迁移指南](https://github.com/angular-ui/bootstrap/wiki/Migration-guide-for-prefixes)。

### CSS

由于最初的Bootstrap的CSS不依赖`href`属性，一些组件（pagination, tabs）的游标风格取决于空的`href`属性。
但是在AngularJS中添加空的href属性在link标签中将会导致有害的路由改变。这就是为什么样式不能正确应用，我们要在指令模板中移除空的`href`属性。这个纠正很简单，只需要在你的应用中增加下面的样式。
	
	.nav, .pagination, .carousel, .panel-title a {cursor: pointer;}
	
### FAQ

普通的问题/解决方法请查看[我们的FAQ](https://github.com/angular-ui/bootstrap/wiki/FAQ)

### 文档阅读

ui-bootstrap提供的每一个组件都有说明文档和可交互的Plunker示例。

对于指令，我们列出不同属性的默认值。除了这个，一些设置还有增加了标记：

* `$watch`-这个设置会被angular $watch 监听。
* `B`-这个设置是布尔值，它不需要参数。
* `C`-这个设置是可以在constant service中被配置成全局的形式。
* `$`-这个设置期待一个angular表达式替代文字字符串。如果这个表达式支持布尔值/整数，你可以直接跳过。
* `readonly`-这个设置是只读。

对于服务（你可以通过`$`前缀认出它们），我们会列出所有可能的参数，你可以跳过它们，和它们默认的值。
* 有些指令会有一个配置服务，就像这种模式：`uibDirectiveConfig`. 这个服务设置使用驼峰式命名。服务可以在实例的`.config`方法中配置。

### Accordion
### Alert
### Buttons
### Carousel
### Collapse
### Dateparser
### Datepicker
### Datepicker Popup
### Dropdown 
### Modal
### Pager
### Pagination
### Popover
### Position
### Progressbar
### Rating
### Tabs
### Timepicker
### Tooltip
### Typeahead
