---
title: 'Redis无法退出：Failed opening .rdb for saving: Permission denied'
date: 2018-01-05 18:56:51
tags: Redis
categories: log
---

* 系统环境 Mac OS
* Redis 3.0.3

使用命令`redis-server`启动Redis , 退出时出现错误，导致无法结束程序

```bash
4901:signal-handler (1515149226) Received SIGINT scheduling shutdown...
4901:M 05 Jan 18:47:06.515 # User requested shutdown...
4901:M 05 Jan 18:47:06.515 * Saving the final RDB snapshot before exiting.
4901:M 05 Jan 18:47:06.516 # Failed opening .rdb for saving: Permission denied
4901:M 05 Jan 18:47:06.516 # Error trying to save the DB, can't exit.
```

原因：退出程序时，Redis要将内存中的数据保存到磁盘中，却没有权限操作相关文件，这个文件是`redis.conf`配置文件中， `dir` 字段后面对应的文件夹名，和 `dbfilename`字段后面对应的文件名，`redis.conf`文件通常在redis安装目录

解决方案，使用`chmod`修改文件夹权限，使当前用户有写入权限即可

[参考 stackoverflow ](!https://stackoverflow.com/questions/22160753/redis-failed-opening-rdb-for-saving-permission-denied)

