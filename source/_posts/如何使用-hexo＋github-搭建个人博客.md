---
title: 如何使用 hexo＋github 搭建个人博客
abbrlink: bb827274
date: 2017-11-22 16:54:08
tags:
---

GitHub Pages 有以下几个优点：

- 轻量级的博客系统，没有麻烦的配置
- 免费空间，享受 Git 版本管理功能
- 使用标记语言，比如 Markdown
- 无需自己搭建服务器
- 可以绑定自己的域名

当然他也有缺点：

- 搭配模板系统，相当于静态页发布，每运行生成一次都必须遍历全部的文本文件，网站越大，生成时间越长
- 动态程序的部分相当局限，比如没有评论，不过有解决方案
- 基于 Git，很多东西需要定制，不像 Wordpress 有强大的后台

要想搭建漂亮的 blog，还需要模板系统，官方推荐的是 jekyll，但是配置稍复杂，今天推荐另一个选择 —— hexo，一个简单地、轻量地、基于 Node 的一个静态博客框架。

下面介绍下如何使用 hexo 和 github pages 搭建个人博客。

# 准备工作

注意: 本文针对 Windows 平台和 Hexo 3.1.1

## 安装 github windows

主要使用 git bash，如果对 git 命令不熟悉的也可以使用 git 客户端进行某些操作
[github windows](https://desktop.github.com/)

## 安装 node.js

因为要使用 npm，比较简单的方法就是安装 node.js
[node.js](https://nodejs.org/en/)
安装完成后添加 Path 环境变量，使 npm 命令生效
`;C:\Program Files\nodejs\node_modules\npm`

## 创建 Github Pages

没有 github 账号的话，需要注册一个，不赘述

然后创建一个仓库，名字是[yourGithubAccount].github.io

## 配置 ssh key

使用 git bash 生成 public ssh key，以下是最简单的方法

```
$ssh-keygen -t rsa
```

C/Documents and Settings/username/.ssh 目录下会生成 id_rsa.pub

将 id_rsa.pub 的内容完全复制到 github Account Setting 里的 ssh key 里即可

## 测试

```
$ssh -T git@github.com
```

然后会看到

```
Hi [yourGithubAccount]! You've successfully authenticated, but GitHub does not provide ps access.
```

## 设置用户信息

```
$git config --global user.name "[yourName]"//用户名
$git config --global user.email  "[yourEmail]"//填写自己的邮箱
```

经过以上步骤，本机已成功连接到 github，为部署打下基础。

# 配置 hexo

## 本地 clone

创建本地目录，然后使用 git bash 或者客户端 clone 之前创建的仓库（[yourGithubAccount].github.io）

## 安装、配置 hexo

- 进入仓库目录，使用 git bash 安装配置 hexo

  ```
  $npm install -g hexo-cli
  $npm install hexo --save
  $hexo init
  ```

- 安装 hexo 插件

  ```powershell
  $npm install hexo-generator-index --save
  $npm install hexo-generator-archive --save
  $npm install hexo-generator-category --save
  $npm install hexo-generator-tag --save
  $npm install hexo-server --save
  $npm install hexo-deployer-git --save
  $npm install hexo-deployer-heroku --save
  $npm install hexo-deployer-rsync --save
  $npm install hexo-deployer-openshift --save
  $npm install hexo-renderer-marked@0.2 --save
  $npm install hexo-renderer-stylus@0.2 --save
  $npm install hexo-generator-feed@1 --save
  $npm install hexo-generator-sitemap@1 --save
  $npm install hexo-wordcount --save
  ```

- 安装 ejs，否则无法解析模板

  ```
  $npm install
  ```

- 生成 public 文件夹

  ```
  $hexo g
  ```

- 浏览器输入 localhost:4000 本地查看效果

  ```
  $hexo s
  ```

- 主题
  hexo 有很多主题可选，我选了 Jacman，默认支持多说评论、网站统计、分享等功能，只要稍微配置即可使用。可以根据自己需求进行选择。

# 使用 hexo

## 部署

配置 _config.yml

```
deploy:
  type: git
  repository: git@github.com:[yourGithubAccount]/[yourGithubAccount].github.io.git
  branch: master
```

```
$hexo d
```

即可将 hexo 部署到 github 上

提示找不到 git 时
需执行（虽然之前已经执行过）

```
$npm install hexo-deployer-git --save
```

然后

```
$hexo d
```

即可通过 [http://[yourGithubAccount\].github.io/](http://[yourgithubaccount].github.io/) 查看了

## 发表新文章

```
$hexo new "title"
```

然后在 source/_post 下会生成该md文件，即可使用编辑器编写了

编写过程中，可以在本地实时查看效果，很是方便

支持 markdown，不了解的自行 google 吧

编写完成后，部署还是一样的

```
$hexo g
$hexo d
```

如果部署过程中报错，可执行以下命令重新部署

```
$hexo clean
$hexo generate
$hexo deploy
```

## 添加自定义页面

```
$hexo new page "about"
```

该命令会生成 source/about/index.md，编辑即可

## 插件的升级与卸载

```
$npm update
$npm uninstall <plugin-name>
```

## 更新 hexo

```
$npm update -g hexo
```

## 绑定自定义域名

在/source/ 目录下新建内容为自定义域名的 CNAME 文件，部署即可（域名设置略）

## 备注：

Hexo简写命令

```powershell
$hexo n #new
$hexo g #generate
$hexo s #server
$hexo d #deploy
```

以上是基本操作，质量高的 blog 所带来的好处是不言而喻的，感兴趣的可以行动起来了。

# 参考资料

<http://alfred-sun.github.io/blog/2014/12/05/github-pages/>
<http://beiyuu.com/github-pages/>
<http://cnfeat.com/2014/05/10/2014-05-11-how-to-build-a-blog/>
<http://wsgzao.github.io/post/hexo-guide/>
<http://fy98.com/2014/03/03/build-blog-with-hexo/>
