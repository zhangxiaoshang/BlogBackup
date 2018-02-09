---
title: 使用Vue官方测试工具和Jest进行Vue.js组件单元测试
tags:
---
# 使用Jest写第一个Vue.js组件单元测试

学习如何使用VueJS官方工具和Jest框架写单元测试  

[vue-test-utils](https://github.com/vuejs/vue-test-utils),是以[avoriaz](https://github.com/eddyerburgh/avoriaz)为基础的官方VueJS测试库，[@EddYerburgh](https://twitter.com/EddYerburgh)创建了它确实是一件很棒的工作。它提供了所有必需的工具，让我们可以很容易的对VueJs应用写单元测试。

另一方面，[Jest](https://facebook.github.io/jest/),这个测试框架是由Facebook开发，它的一些很赞的功能使得测试轻而易举，比如：

* 几乎不需要配置，默认的就可以  
* 很酷的接口  
* 测试并行运行
* Spies,stubs and mocks 开箱即用
* 构建代码覆盖率
* 快照测试
* 模块模拟工具

也许你已经用一些其他的工具写过测试，比如 karma + mocha + chai + sinon + ..., 现在，你将看到使用Jest是多么容易。

## 建立一个简单的vue-test项目

使用vue-cli创建一个新项目，对于所有yes/no的问题选择NO

下面是我创建项目时终端的打印信息，可以作为参考

```bash
$ vue init webpack vue-test

? Project name vue-test
? Project description A Vue.js project
? Author zhangxiaoshang<zhangxiaoshang66@163.com>
? Vue build standalone
? Install vue-router? No
? Use ESLint to lint your code? No
? Set up unit tests Yes
? Pick a test runner jest
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recommended) npm

   vue-cli · Generated "vue-test".


# Installing project dependencies ...
# ========================
.........
# Project initialization finished!
# ========================

To get started:

  cd vue-test
  npm run dev
```

然后我们需要安装一些依赖：

```bash
# Install dependencies
npm i -D jest jest-vue-preprocessor babel-jest
```
