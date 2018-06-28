---
title: 如何在Ubuntu 18.04上安装Nginx（译文）
date: 2018-06-28 23:16:44
updated: 2018-06-28 23:16:44
tags: Nginx
categories: note
toc: true
---
阅读原文 [How To Install Nginx on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04#)

本教程以前的版本由 [Justin Ellingwood](https://www.digitalocean.com/community/users/jellingwood)编写

## 介绍
Nginx是世界上最流行的web服务器之一，它负责托管互联网上一些大型、高流量的站点。它比Apache更加资源友好，在多数情况下可以作为web服务器或者反向代理。

在该指南中，我们将讨论如何在你的Ubuntu 18.04服务器上安装Nginx

## 预备条件
在开始本指南前，你应该为你的服务器配置普通的、拥有sudo权限的非root用户。你可以通过我们的[initial server setup guide for Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04)学习如何配置普通用户账户.

当你拥有可用的用户时，请以你的非root用户身份登录以开始。

## 第一步-安装Nginx

因为Nginx在Ubuntu默认仓库中是可用的，所以可以使用`apt`从这些仓库中安装它。

因为这是我们在这次会话中与apt的首次交互，我们将更新我们本地的包索引，以至于我们我们能够访问最新的软件包列表。之后我们就可以安装nginx:

```bash
sudo apt update
sudo apt install nginx
```

在接收该过程之后，apt会将Nginx和任何所需要的依赖安装到你的服务器上。

## 第二步-调整防火墙
开始测试Nginx之前，防火墙需要做一些调整已允许访问这个服务。Nginx在安装时将自己注册为`ufw`服务，使它允许被Nginx直接访问。

通过键入如下，ufw会列出应用程序配置：
```bash
sudo ufw app list
```
你应该会获得一份应用程序配置文件列表：
```bash
# Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
如你所见，这有三个Nginx的可用配置文件：
* Nginx Full: 此配置文件打开端口80（常规，未加密的web流量）和端口443（TLS/SSL加密流量）
* Nginx HTTP: 这个配置文件只打开端口80（常规，未加密的web流量）
* Nginx HTTPS: 这个配置文件只打开端口443（TLS/SSL加密流量）

建议启用最严格的配置文件，该配置文件仍允许你已经配置的流量。因为在本指南中我们尚未在我们的服务器上配置SSL，所以我们只需允许在80端口上的流量。

你可以键入如下启用：
```bash
sudo ufw allow 'Nginx HTTP'
```
你可以像这样验证变化：
```bash
sudo ufw status
```
你可以在输出信息中看到HTTP流量已经被允许了：

```bash
# Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

## 第三步-检测你的Web服务器

在安装过程结束后，Ubuntu 18.04开启了Nginx。这个Web服务器应该已经启动并运行了。

我们可以通过`systemd`检查以确保服务正在运行：

```bash
systemctl status nginx
```

```bash
#Output
nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2018-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```

如你在上面所见，这个服务好像已经成功启动。但是，最好的验证方式是从Nginx上实际请求一个页面。

通过在浏览器中访问你服务器的IP地址，你可以访问到一个Nginx的默认页面以确定Nginx软件在正确运行。如果你不知道自己服务器的IP地址，你有以下几种不同的方式查看：

尝试在你服务器的终端中键入：

```bash
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```

你可以得到几行反馈，你可以在浏览器中依次尝试，观察它们是否可以工作。

另一种方法是键入下面信息，它将向你提供你的公开IP地址，可以在互联网上的其他地方访问的地址。

```bash
curl -4 icanhazip.com
```

当你获取了服务器IP地址，在你的浏览器地址栏中访问：

```bash
http://your_server_ip
```

你会看到默认的Nginx页面：

![Welcome to nginx!](https://assets.digitalocean.com/articles/nginx_1604/default_page.png)

这是Nginx附带的页面，用于向你展示服务器在正确运行。

## 第四步-管理Nginx进程

现在你的web服务器已启动并正在运行，让我们回顾一些基础的管理命令。

停止web服务器：

```bash
sudo systemctl stop nginx
```

当服务器停止的时候启动web服务器:

```bash
sudo systemctl start nginx
```

重启web服务器：

```bash
sudo systemctl restart nginx
```

如果只是简单的进行配置修改，Nginx通常可以重新加载而不会丢失连接：

```bash
sudo stystemctl reload nginx
```

Nginx的默认配置是，当服务器启动时Nginx会自动启动，如果这不是你想要的，你可以禁用这一行为：

```bash
sudo systemctl disable nginx
```

想要重新启用这一服务时（译注：和上一步相对）：

```bash
sudo systemctl enable nginx
```

## 第五步-设置服务器模块

在使用Nginx web服务器时，可以使用服务器模块封装配置详细信息并在单个服务器上托管多个域。我们将建立一个名为`example.com`的域名，但是你应该替换成自己的域名。在DigitalOcean了解更多关于配置域名的信息，可以看我们的[Introduction to DigitalOcean DNS](https://www.digitalocean.com/docs/networking/dns/).

Ubuntu 18.04上的Nginx默认启用了一个服务器模块，该服务器模块配置的服务文件路径为`/var/www/html`. 这适用于单个站点，如果托管多个站点可能会变得难以处理。我们不必修改 `/var/www/html`, 而是在 `/var/www` 中为我们的 `example.com` 网站创建一个目录结构，并将 `/var/www/html` 保留为默认目录，如果客户端请求没有匹配任何其他网站。

为 `example.com` 创建目录，使用 `-p` 标志，可以创建必要的父目录：

```bash
sudo mkdir -p /var/www/example.com/html
```

下一步，使用 `$USER` 环境变量分配目录的所有者权限：

```bash
sudo chown -R $USER:$USER /var/www/example.com/html
```

如果你没有修改你的 `umask(译注：说的是掩码吧)` 值，你web根目录的权限应该是正确的，你可以这样确保：

```bash
sudo chmod -R 755 /var/www/example.com
```

下一步，使用 `nano` 或者其他你喜欢的编辑器创建一个简单的 `index.html` 页面:

```bash
nano /var/www/example.com/html/index.html
```

添加一些简单的HTML：

```HTML
<html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

完成之后保存并关闭这个文件。

为了让Nginx能够提供这些内容，创建一个有正确指令的服务器模块是必要的。我们不必修改默认的配置文件，而是在 `/etc/nginx/sites-available/example.com` 上创建一个新的配置文件：

```bash
sudo nano /etc/nginx/sites-available/example.com
```

粘贴以下内容到配置块中，这些信息类似于默认值，但已更新为我们但新目录和域名：

```bash
server {
        listen 80;
        listen [::]:80;

        root /var/www/example.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name example.com www.example.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

请注意，我们已经将根目录更新到我们的新目录，并将 `server_name` 更新为我们的域名。

接下来，让我们创建一个链接到 `sites-enabled` 目录，该目录是Nginx在启动过程中读取的：

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

现在配置并启用了两个服务器模块，以基于它们的 `listen` 和 `server_name` 指令响应请求（你可以在[这里](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)了解更多关于Nginx如何处理这些指令）

* example.com: 将响应请求 `example.com` 和 `www.example.com`.
* default: 将响应端口80上任何与其他两个块不匹配的请求.

为了避免添加额外的 server names 可能出现的 hash bucket memory 问题，有必要调整 `/etc/nginx/nginx.conf`文件中的单个值。打开文件:

```bash
sudo nano /etc/nginx/nginx.conf
```

找到 `server_names_hash_bucket_size` 指令，删除 `#` 符号以取消对该行的注释:

```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

接着，测试以确认在你的Nginx文件中没有语法错误：

```bash
sudo nginx -t
```

完成之后保存并关闭文件。

如果没有任何问题，重启Nginx以启用更改：

```bash
sudo systemctl restart nginx
```

Nginx现在应该为你的域名提供服务，你可以通过导航至 `http://example.com` 测试这一点，然后你应该看到类似这样的内容：

![example.com](https://assets.digitalocean.com/articles/nginx_server_block_1404/first_block.png)

## 第六步-熟悉重要的Nginx文件和目录

既然你已经知道如何管理Nginx本身，你应该花几分钟时间熟悉一些重要的目录和文件。

### 内容

* `/var/www/html`: 默认情况下，实际的网页内容仅包含你之前看到的默认Nginx页面，它在 `/var/www/html` 目录中提供，这可以通过修改Nginx配置文件来改变。

### 服务器配置

* `/etc/nginx`: Nginx配置目录。所有Nginx配置文件都在这里。
* `/etc/nginx/nginx.conf`: Nginx主配置文件。这个可以修改，以更改Nginx全局配置。
* `/etc/nginx/sites-availabel/`: 存储每个站点服务器块的目录。除非配置文件被链接到 `sites-enabled` 目录，否则Nginx不会使用此目录中的配置文件。通常，所有服务器块配置都在此目录中完成，然后通过链接到其他目录启用。
* `/etc/nginx/sites-enabled/`: 启用的每个站点服务器模块目录。通常，它们是 `sites-available` 目录下文件的链接。
* `/etc/nginx/snippets`: 该目录包含可以包含在其他任何Nginx配置中的配置片段。可重复的配置片段最好重构成配置片段。

### 服务器日志

* `/var/log/nginx/access.log`: 除非Nginx配置为其他方式，否则每个对Web服务器的请求都会记录在这个日志文件中。
* `/var/log/nginx/error.log`: 任何Nginx错误都会被记录在这个日志中。

## 总结

现在你安装了你的web服务器，你有不同类型的服务内容供选择，还可以使用你想用的技术创建丰富的体验。

如果你像建立一个更完整的应用程序栈，可以查看这篇文章[how to configure a LEMP stack on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04).
