---
title: Hello Hexo Again
categories: log
tags: Hexo
date: 2017-03-01 09:37:12
---
>是不是很有极客范儿, 我喜欢这个博客!

### hexo搭建笔记

1.	安装hexo。
2.	拷贝_config.yml文件，这是hexo的配置文件，保存了博客首页设置、git部署路径等配置。
3. 拷贝themes文件夹下的themes/cactus-dark文件夹替换原有主题。


		至此，hexo重新搭建完成，且主题、配置等和原有设置一致！
		如果使用：hexo deploy 命令时出错：ERROR Deployer not found: git
		解决办法：npm install hexo-deployer-git --save
		
### 2017-04-16 增加浏览量统计

1.引入**卜蒜子**

	<script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
这段代码可以写在footer.ejs、header.ejs、layout.ejs, 我写在了footer.ejs文件里。

	 .../themes/(myThemes)/layout/_partial/footer.ejs

2.增加站点访问量

修改文件

	.../themes/(myThemes)/layout/_partial/footer.ejs

添加代码

```html
<span id="busuanzi_container_site_uv"> 
  本站访客数<span id="busuanzi_value_site_uv"></span>人次
</span>
```
计算访问量的方法有两种：

1. 算法a：pv的方式，单个用户连续点击n篇文章，记录n次访问量。
2. 算法b：uv的方式，单个用户连续点击n篇文章，只记录1次访客数。
我用的是uv的方式，自行选择即可。

3.添加文章访问量

修改文件

	.../themes/(myThemes)/layout/_partial/post/date.ejs

添加代码

```html
	<span id="busuanzi_container_page_pv">
		浏览 <span id="busuanzi_value_page_pv"></span>
	</span> 
```

### 2017-06-03 重新搭建博客总结

1. 安装hexo

	1. `$ npm install -g hexo-cli`
	2. `$ hexo init <folder>`
	3. `$ cd <folder>`
	4. `$ npm install`

2. 使用原始文件/夹替换搭建hexo生成的初始文件/夹，包括：
	
	1. scaffolds (模板)
	2. source (文章原始数据，重要！)
	3. themes (主题)
	4. _config.yml (配置)

3. 	`$ npm install hexo-deployer-git --save` （执行这条命令，否则使用 `hexo deploy`部署时会报错.）

### 2018-01-17 新主题增加GitCommnet评论系统

1. themes/yilia/_config.yml 配置相应参数即可



