---
title: NexT主题设置
comments: true
date: 2017-11-22 18:03:17
categories:
tags:
---

# NexT字数统计与阅读时长设置

NexT主题默认已经集成了文章【字数统计】、【阅读时长】统计功能，如果我们需要使用，只需要在主题配置文件(Blog\themes\next_config.yml)中打开`wordcount` 统计功能即可。

## 问题

如果仅仅只是打开开关，部署之后会发现文章的【字数统计】和【阅读时长】后面没有对应的xxx字，xx分钟等字样，只有光秃秃的数字在那里。

<!-- more -->

## 解决方案

找到`Blog\themes\next\layout\_macro\post.swig` 文件

- 字数统计

搜到找到如下代码：

```html
<span title="{{ __('post.wordcount') }}">
     {{ wordcount(post.content) }}
</span>
```

添加 “字”到`{{ wordcount(post.content) }}` 后面，修改后为

```html
<span title="{{ __('post.wordcount') }}">
     {{ wordcount(post.content) }} 字
</span>
```

- 阅读时长

找到如下代码

```html
<span title="{{ __('post.min2read') }}">
   {{ min2read(post.content) }}
</span>
```

添加 “分钟”到`{{ min2read(post.content) }}` 后面，修改后为：

```html
<span title="{{ __('post.min2read') }}">
   {{ min2read(post.content) }} 分钟
 </span>
```

再次运行，就能得到正常的如“字数统计 xxxx字” 、 “阅读时长 xx分钟”这样的样式了.



# 设置分类列表

## 修改根目录下_config.yml

```
# Directory
source_dir: source
public_dir: public
#这个是tags的目录
tag_dir: tags
archive_dir: archives
#这个是category的目录
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
```

## 修改themes下_config.yml

```
menu:
  home: /
  #categories要打开
  categories: /categories
  #about: /about
  archives: /archives
  #tags要打开
  tags: /tags
  #sitemap: /sitemap.xml
  #commonweal: /404.html
```

## 生成tags和categories页面

在根目录下执行

```
$hexo n page "tags"
$hexo n page "categories"
```

然后去source目录下找到categories/index.md
修改：

```
---
title: categories
date: 2017-04-19 16:19:25
type: categories
comments: false
---
```

去source目录下找到tags/index.md
修改：

```
---
title: tags
date: 2017-04-19 16:19:25
type: tags
comments: false
---
```

这样就配置好了

# 在文章中添加tag、分类

## 模板中添加

编辑 /sacaffolds/post.md

```
---
title: {{ title }}
date: {{ date }}
categories: 
tags:
---
```

这样，new出来的文章默认就带上categories和tags了

## 多个tag的写法

```
tages: 
    - 标签1
    - 标签2
    ...
    - 标签n
```


# 链接持久化终极解决之道

## 使用hexo-abbrlink插件

### 安装插件

```powershell
$ npm install hexo-abbrlink --save
```

### 修改站点配置文件

```yaml
permalink: post/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

### 修改`scaffolds`里的模版文件

修改`post.md`为:

```yaml
---
title: {{ title }}
date: {{ date }}
comments: true
categories:
tags:
---1234567
```
