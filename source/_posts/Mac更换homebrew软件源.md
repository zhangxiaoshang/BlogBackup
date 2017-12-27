---
title: Mac OS 更换Homebrew软件镜像源
categories: log
tags: [Mac, MySql]
date: 2017-09-02 23:13:24
update: 2017-12-27 09:08
---
### SOMETHING

* Device

    * MacBook air
    
* OS X version

    * 10.12.3 (16D30)
    
* Brew --version

    * Homebrew 1.3.1

* Dependencies

    * git


### DO

```shell
# 替换brew.git:
$ cd "$(brew --repo)"
# 清华大学:
$ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
```

```shell
# 替换homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
# 清华大学:
$ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
```

```shell
# 替换homebrew-bottles:
# 清华大学:
$ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
$ source ~/.bash_profile
```

```shell
# 应用生效:
$ brew update
```
[参考文献 http://blog.csdn.net/u010275932/article/details/76080833](http://blog.csdn.net/u010275932/article/details/76080833)

---

Homebrew官方更新源是大陆访问一直是比较慢的，之前凑合着用，直到今儿，本来打算晚上搞事儿，结果一个mysql愣是耗了一个晚上才装上，可能也和郊区的渣网有关，不能忍，试了几次，最后成功替换为清华大学提供的软件源。

---

### Tip 

Mac系统下使用`brew install mysql`安装`mysql`后默认`mysql`是未启动的，直接运行`mysql -uroot`会出错：

    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
    
解决办法，就是启动mysql：

```shell
$ mysql.server start
```

出现

    Starting MySQL
    . SUCCESS!
    
表示mysql启动成功，这时候就可以继续使用`mysql -uroot`登陆到mysql, (使用 brew install mysql 命令安装的mysql默认root用户密码为空)

可以为`root`用户设置个密码：

```shell
$ mysqladmin -uroot password root  #设置root的密码为 root 
```

因为`root`用户一开始是没有密码的，所以省去了`-p`参数。

