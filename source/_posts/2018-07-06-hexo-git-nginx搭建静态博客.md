---
layout: "post"
title: "hexo+git+nginx搭建静态博客"
date: "2018-07-06 17:49"
tag:
  - hexo
  - git
  - nginx
---

## 实施方案

### 方案一

* 在本地（服务器或个人电脑）安装hexo
* 使用hexo-server模块将博客发布在本地

### 方案二

* 在本地部署hexo
* 使用hexo-deployer-git模块将博客提交到github特定的项目
* 使用github-pages发布博客

### 方案三

* 在本地部署hexo
* 在服务器端部署git资源库服务
* 使用hexo的hexo-deployer-git模块将博客发布到服务器端的git资源库
* 使用git-hooks将资源库的变更同步到服务器端nginx网站目录
* 使用nginx发布博客

### 部署环境

* 服务端OS：CentOS release 6.9 (Final) - 64BIT
* 客户端OS：Windows 10
* nodejs：4.8.4
* hexo：3.3.8
* git：2.12.0.windows.1 / 1.7.1 (CentOS)
* nginx：1.10.2

## 方案一实施

### 在本地（服务器或个人电脑）安装hexo

[安装nodejs](../2018-07-06-在windows下使用nvmw管理nodejs版本/)。

执行以下命令安装hexo-cli：

``` bash
> npm install -g hexo-cli
```

创建并初始化hexo项目目录：

``` bash
> hexo init blog
```

安装hexo-server模块，用于在本地简单地发布博客：

``` bash
> npm install hexo-server
```

### 发布静态博客

将.md文件渲染成静态文件，然后启动服务：

``` bash
> hexo g
> hexo server
```

Linux在后台运行则执行以下语句：

``` bash
# nohup hexo server -p 4000 &
```

现在便可以打开浏览器访问`http://<ip>:4000`来查看已发布的博客了。

## 方案二实施

### 在本地安装hexo

[安装nodejs](../2018-07-06-在windows下使用nvmw管理nodejs版本/)。

安装hexo并创建hexo项目。

等执行成功以后安装两个模块，hexo-deployer-git和hexo-server，这俩模块的作用分别是使用git自动部署和本地简单的服务器。

``` bash
> npm install hexo-deployer-git --save
> npm install hero-server
```

### 自动部署

在github账户新建资源库`<用户名>.github.io`。

然后打开_config.yml，找到`deploy`节点。

``` bash
deploy:
    type: git
    repo:  git@github.com:/<account>/<account>.github.io.git    //<repository url>
    branch: master            //这里填写分支   [branch]
    message: 提交的信息         //自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```

保存后，尝试将我们的页面部署到服务器。

``` bash
> hexo clean
> hexo generate --deploy
```

访问服务器地址，就可以看我们部署的博客了，以后更新博客只需要下列操作：

``` bash
> hexo new "Blog article name"
## 编辑文章
> hexo clean && hexo generate --deploy
```

## 方案三实施

### 在服务器端部署git

安装git，参[git的安装与使用说明](../2018-07-06-git的安装与使用说明/)。

``` bash
# yum install git
```

创建git用户。

``` bash
# adduser git
# chmod 740 /etc/sudoers
# vim /etc/sudoers
```

找到一下内容。

``` bash
## Allow root to run any commands anywhere
root    ALL=(ALL)   ALL
```

在下面添加一行。

``` bash
git     ALL=(ALL)   ALL
```

保存退出后改回权限。

``` bash
# chmod 400 /etc/sudoers
```

随后设置git用户的密码。

``` bash
## 需要root权限
# passwd git
```

切换至git用户，创建`~/.ssh`文件夹和`~/.ssh/authorized_keys`文件，并赋予相应的权限。

``` bash
# su git
$ mkdir ~/.ssh
$ vim ~/.ssh/authorized_keys
## 然后在本地执行 cat ~/.ssh/id_rsa.pub | pbcopy，将公钥复制粘贴到authorized_keys
$ chmod 600 ~/.ssh/authorzied_keys
$ chmod 700 ~/.ssh
```

然后就可以执行ssh命令测试是否可以免密登录。

``` bash
$ ssh -v git@<ip/domain>
```

至此，git用户添加完成。

### 在服务器端部署nginx服务

找到nginx的配置文件，修改配置如下：

``` bash
server
{
    listen 80;
    #listen [::]:80;
    server_name www.seekbetter.me seekbetter.me;
    index index.html index.htm index.php default.html default.htm default.php;
    #这里要改成网站的根目录
    root  /path/to/www;  

    include other.conf;
    #error_page   404   /404.html;
    location ~ .*\.(ico|gif|jpg|jpeg|png|bmp|swf)$
    {
        access_log   off;
        expires      1d;
    }

    location ~ .*\.(js|css|txt|xml)?$
    {
        access_log   off;
        expires      12h;
    }

    location / {
        try_files $uri $uri/ =404;
    }

    access_log  /home/wwwlogs/blog.log  access;
}
```

### 在本地安装hexo

[安装nodejs](../2018-07-06-在windows下使用nvmw管理nodejs版本/)。

安装hexo并创建hexo项目。

等执行成功以后安装两个模块，hexo-deployer-git和hexo-server，这俩模块的作用分别是使用git自动部署和本地简单的服务器。

``` bash
> npm install hexo-deployer-git --save
> npm install hero-server
```

### 自动部署

在服务器端创建一个裸仓库，裸仓库就是只保存git信息的Repository, 首先切换到git用户确保git用户拥有仓库所有权

一定要加`--bare`，这样才是一个裸库。

``` bash
$ su git
$ cd ~
$ git init --bare blog.git
```

在这里我们使用的是post-receive这个钩子，当git有收发的时候就会调用这个钩子。

在~/blog.git裸库的hooks文件夹中，新建post-receive文件。

``` bash
$ vim ~/blog.git/hooks/post-receive

#!/bin/sh
git --work-tree=/path/to/www --git-dir=~/blog.git checkout -f
```

保存后，要赋予这个文件可执行权限。

``` bash
$ chmod +x post-receive
```

然后打开_config.yml，找到deploy。

``` bash
deploy:
    type: git
    repo: git@<ip/domain>:/home/git/blog.git    //<repository url>
    branch: master            //这里填写分支   [branch]
    message: 提交的信息         //自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```

保存后，尝试将我们刚才写的”hello hexo”部署到服务器。

``` bash
$ hexo clean
$ hexo generate --deploy
```

访问服务器地址，就可以看到我们写的文章”Hello hexo”，以后更新博客只需下列操作：

``` bash
$ hexo new "Blog article name"
## 编辑文章
$ hexo clean && hexo generate --deploy
```
