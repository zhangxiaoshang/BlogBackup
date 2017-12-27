---
title: JavaScript正则表达式
categories: note
tags: JS
date: 2017-04-11 09:37:12
---
### 1-1 JS正则表达式简介及工具介绍

#### 课程目标
* 了解正则表达式语法
* 在IDE中使用正则表达式处理规则复杂的字符串查找、替换需求
* 在JavaScript程序设计中使用正则表达式处理字符串

#### 什么是正字表达式?

* Regular Expression 使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。

* 说简单了就是按照某种规则去匹配符合条件的字符串。

举个例子
>我们在linux系统中，想要查找当前目录中以txt为后缀的文件时，可以使用命令：**find ./ -name *.txt** 这里 **\*** ；代表任意字符，这就使用了正则表达式。

然而，并不是所有的正则表达式都这么简单，有的正则表达式会非常复杂，比如这个

	^([a-zA-Z0-9\_-])+@([a-zA-Z0-9\_-])+((\.[a-zA-Z0-9_-]{2,3}){1,2}$ 

就不太容易看出所匹配的内容，这时候我们可以借助图形化工具帮助理解正则表达式意图，这里推荐一款在线的可以图形化正则表达式的工具，[Regexper 正则表达式图形化工具](https://regexper.com/)
我们将刚才的正则表达式借助图形化工具转化后就是这种样式:
![regexper](http://oigrj4b52.bkt.clouddn.com/image/github/blog/20170126/regexpreg_email.png)
很容易可以看出这是一个匹配邮箱地址的正则表达式！

再比如：

**/d** 表示匹配数字

![digit](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/reg-digit.png)

**/d?** 表示匹配0或1个数字，**?**表示0或1个

![digit0or1times](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/gitit0or1times.png)

**/d+** 表示匹配至少一个数字，**+**表示至少1个

![digitmore1times](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/digitmore1times.png)

**/d{3}** 表示匹配3个数字，**{3}**是量词，表示3个

![digit3times](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/digit3times.png)

**/d{3,5}** 表示匹配3~5个数字，**{3，5}**表示范围，3~5个

![digit3to5](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/digit3to5.png)

**/d\*** 表示匹配任意个数字，**\***表示任意个，也可以是0个

![digitany](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/digitany.png)

正则表达式的简单应用场景

比如我们有一个文本文件，内容下图：

![testtext](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/testtext.png)

我们想要把文本中的**is**全部替换成**IS**，我们可以使用编辑器的查找/替换功能进行全部替换，替换完成后将得到如图所示文本数据：

![textResult](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/testresult.png)

我们发现，不仅单词is被替换了，this、display、isn’t中的is也被替换了，这并不是我们期待的结果，如何做到只替换单词is而忽视其他非单独单词的is部分？这时就可以使用正则表达式来达到期待的结果（其实，这个不使用正则也是可以实现的，不过我们还是说说正则的实现方式）:

![wholeworld](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/wholeworld.png)

这里使用**\b**表示单词边界，这样只有当is为单个单词的时候才会匹配！

第二个例子，日期转换

我们想要把日期**2017/01/26**转换成**26/01/2017**这种格式，即，将年月日转换成日月年格式

转换前:
![date](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/dateold.png)

表达式**(\d{4})-(\d{2})-(\d{2})**将匹配**2017-01-26**，并将每个小括号的内容分为一组，如图

![dategroup](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/dategroup.png)

然后我们使用分组进行替换，第一组匹配到的内容用$1表示，第二组用$2表示...

转换后：
![date](http://oigrj4b52.bkt.clouddn.com/image/github/blog/regexp/20170126/datenew.png)

通过这几个小例子，对正则表达式进行了简单的介绍，后面会对正则表达式进行详细的语法讲解。

### 2-1 RegExp对象

JavaScript通过内置对象RegExp支持正则表达式；

有两种方法实例化RegExp对象

* 字面量
* 构造函数

先来看一下使用字面量的方式，还是之前匹配**is**的例子

```js
var reg = /\bis\b/;
```

同样的，使用构造函数方式如下

```js
var reg = new RegExp('\\bis\\b');
```

这里反斜线使用了两次，是因为在js中反斜线本身就是特殊字符，使用的时候需要转义。

#### 修饰符

* g: global 全文搜索，不添加，搜索到第一个匹配值后就会停止向下继续匹配
* i: ignore case 忽略大小写，默认大小写敏感
* m: multiple lines 多行搜索

举个例子

```js
'He is a boy. Is he?'.replace(/\bis\b/g, 'xx');  // g 表示进行全局匹配
// 输出结果: He xx a boy. Is he?
```

这是因为在js中，正则表达式默认大小写敏感，如果想忽略大小写则

```js
He is a boy. Is he?'.replace(/\bis\b/gi, 'xx');	// i 表示忽略大小写
// 输出结果：He xx a boy. xx he?
```
### 2-2 元字符

正则表达式由两种基本的字符类型组成

* 原义文本字符
* 元字符

**原义文本字符**就是代表它本来含义的字符，比如：a,b,c,1,2,3等等；

**元字符**是指在正则表达式中有特殊含义的非字母字符，比如我们之前用到的**\b**表示单词边界，**\d**表示数字

在正则表达式中有几个字符是需要我们注意的，它们都有着特殊含义
	
	* + ? $ ^ . | \ (){}[]

另外还有一些

	\t	水平制表符
	\v	垂直制表符
	\n	换行符
	\r	回车符
	\0	空字符（这个是数字零）
	\f	换页符
	\cX	Ctrl+X

### 2-3 字符类

* 一般情况下正则表达式的一个字符对应字符串一个字符，如'a'就表示字符'a';
* 我们可以使用元字符**[]**来构建一类字符，比如[abc]就可以匹配a或b或c；
* [^abc]表示除a、b、c以外的字符；

### 2-4 范围类

* [a-z] 表示从a到z的任意字符，这是一个闭区间，包含a和z本身；
* [a-zA-Z] 表示任意大小写字母；

### 2-5 JS预定义类及边界

#### 预定义类

| 字符 | 等价类 			| 含义 
 ---  | ------ 			| ----
| .   | [^\r\n]			| 除回车符、换行符之外的所有字符
| \d  | [0-9]  			| 数字字符
| \D  | [^0-9] 			| 非数字字符
| \s  | [\t\n\x0B\f\r] | 空白符
| \S  | [^\t\n\x0B\f\r]| 非空白符
| \w  | [a-zA-Z_0-9]	| 单词字符（字母、数字、下划线）
| \W  | [^a-zA-Z_0-9]	| 非单词字符

#### 单词边界
字符	| 含义
-----	| -----
^		| 以xxx开始
$		| 以xxx结束
\b		| 单词边界
\B 		| 非单词边界

```js
'@123@abc@'.replace(/@/g, 'Q')        // Q123QabcQ
'@123@abc@'.replace(/^@/g, 'Q')       // Q123@abc@
'@123@abc@'.replace(/@$/g, 'Q')         // @123@abcQ
'This is a boy'.replace(/is/g, 'x');	   // Thx x a boy
'This is a boy'.replace(/\bis\b/g, 'x')    // This x a boy
'This is a boy'.replace(/\Bis/b/g, 'x')	   // Thx is a boy
```

#### 多行匹配

```js
// 注释中的 \n 表示换行
var mulStr = "@123\n@456\n@789";		// 使用"\n"转义符添加两个换行
mulStr.replace(/^@/g, 'at');			// at123\n@456\n@789
mulStr.replace(/^@/gm, 'at');			// at123\nat456\nat789
```

### 2-6 量词

假如我们希望匹配一个连续出现20次数字的字符串，我们可能会写成这样的正则表达式：

`\d\d\d\d\d\d\d\d\d\d\d\d\d\d\d\d\d\d\d\d`

类似这种情况，使用量词表示便可简化很多，比如：

`\d{20}`

量词的表示规则如下：

字符		|		含义
----		|		----
？			|		出现零次或一次（最多出现一次）
+			|		出现一次或多次（至少出现一次）
*			|		出现零次或多次（任意次）
{n}			|		出现n次
{n,m}		|		出现n至m次
{n,}		|		至少出现n次 		 

### 2-7 JS正则贪婪模式

`'12345678'.match(/\d{3,6}/);	// ["123456", index: 0, input: "12345678"]`

`\d{3,6}`表示匹配连续出现3至6次数字的字符串，从结果可以知道最终匹配的是连续出现6次数字的字符串，这是因为在js中默认是贪婪匹配，就是尽可能多的匹配；如果想让正则表达式尽可能少的匹配，也即是说一旦成功匹配不再继续尝试就是非贪婪模式，比如：

`'12345678'.match(/\d{3,6}?/);	// ["123", index: 0, input: "12345678"]`


### 2-8 分组
### 2-9 前瞻
### 2-10 JS对象属性
### 2-11 test和exec方法
### 2-12 字符串对象方法