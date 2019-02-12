---
layout: "post"
title: "搭建GitHub Pages个人博客网站"
date: "2019-02-12 14:47"
tags:
  - github
  - hexo
---

## GitHub Pages

GitHub Pages是一个静态网站主机服务，不支持PHP、Ruby、Python等服务端代码，它提供的网页须被管理在GitHub资源库中。

官网上有三点使用指导：
* 创建于2016年6月15日之后并且使用了github.io域名的GitHub Pages网站被提供了HTTPS支持，而之前的网站需要手动激活HTTPS支持。
* GitHub Pages网站不能用于像传送密码或者信用卡号这样的敏感交易。
* GitHub Pages从属于《GitHub服务条款》，不得被拿来转卖。

GitHub Pages网站须遵从如下使用限制：
* GitHub Pages代码库建议限于1GB。
* 发布后的GitHub Pages网站不要大于1GB。
* GitHub Pages网站有每个月100GB的柔性带宽限制。
* GitHub Pages网站有每个小时10次构建的柔性限制。

另外鉴于GitHub不允许被用来做商务活动，有若干网站内容上的限制：
* 服务条款或社区指导不允许或以其他方式禁止的内容或活动。
* 暴力恐怖的内容或活动。
* 过多的自动化批量活动，如垃圾邮件。
* 危害GitHub用户或服务的活动。
* 成功学的内容。
* 淫秽色情的内容。
* 不符合站长身份和网站目的的内容。

## Hexo

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 部署环境

* node.js：10.15.0
* git：2.19.1.windows.1
* hexo-cli：1.1.0
* vscode：1.31.0

## 实施步骤

### 准备GitHub帐号

### 准备本地Hexo开发环境

#### 安装

安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
* [Node.js](../2018-07-06-在windows下使用nvmw管理nodejs版本/)
* [Git](../2018-07-06-git的安装与使用说明/)
如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。
```
> npm install -g hexo-cli
```

#### 建站

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```
> hexo init <folder>
> cd <folder>
> npm install
```
新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

##### _config.yml

网站的 配置 信息，您可以在此配置大部分的参数。

##### package.json

应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。
```
package.json
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": ""
  },
  "dependencies": {
    "hexo": "^3.0.0",
    "hexo-generator-archive": "^0.1.0",
    "hexo-generator-category": "^0.1.0",
    "hexo-generator-index": "^0.1.0",
    "hexo-generator-tag": "^0.1.0",
    "hexo-renderer-ejs": "^0.1.0",
    "hexo-renderer-stylus": "^0.2.0",
    "hexo-renderer-marked": "^0.2.4",
    "hexo-server": "^0.1.2"
  }
}
```

##### scaffolds

模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

Hexo的模板是指在新建的markdown文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。

##### source

资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

##### themes

主题 文件夹。Hexo 会根据主题来生成静态页面。

### 绑定域名

### 个性化Hexo网站

### 使用Hexo的一些约定
