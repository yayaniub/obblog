---
{"dg-publish":true,"dg-home":true,"permalink":"/利用obsidian构建个人博客/","tags":["gardenEntry"],"dgPassFrontmatter":true,"noteIcon":"","created":"2023-12-13T23:24:10.118+08:00","updated":"2023-12-13T23:30:56.199+08:00"}
---

最近，基本实现了迁移个人笔记到obsidian上，同时将笔记也一并迁移过来了。在这也做一个笔记记录一下如何搭建自己的博客和数字花园。

这是我目前的博客效果展示。  
![Pasted image 20230328224521.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328224521.png)

## 准备github库

如同之前使用hexo或者hugo搭建静态博客一样，需要有一个github库作为文件地址，至于这个库是否公开或私人，没有多大影响。因为后面的步骤，我们也会采用vercel类似的服务进行部署，省去在等地搭设环境。

但是本次github和之前有些不一样的要求，之前一般是按照hexo或者hugo项目的要求进行搭建。本次由于使用的是obsidian同步部署，所以得按照插件 [Obsidian-Digital-Garden](https://github.com/oleeskild/Obsidian-Digital-Garden) 的要求，使用 [digitalgarden](https://github.com/oleeskild/digitalgarden) 这个库作为模板，创建自己的库。  
![创建digtalgarden库.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/%E5%88%9B%E5%BB%BAdigtalgarden%E5%BA%93.png)

**相比于hexo,hugo的好处是：创建完digtalgarden库完成后，基本不会再去动这个库的文件，后续大部分的修改都是在obsidian中实现的。额外增加obsidian功能的情况除外。**

## 部署

点击reademe上的deploy按钮，即可对接到vercel服务上，进行自动部署，每天100次部署机会，足够使用了。

## 配置obsidian

### 安装Obsidian-Digital-Garden

在obsidian插件中心安装Obsidian-Digital-Garden插件。  
![安装digitalgarden插件.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/%E5%AE%89%E8%A3%85digitalgarden%E6%8F%92%E4%BB%B6.png)

### 配置插件

repo name: github库名称，自己使用模板创建库时自定义的  
github username: 个人的github昵称  
github token: 在 https://github.com/settings/tokens 里可以创建。保密！保密！保密！  
base url： 个人的博客主页  
note settings：建议全开  
appearance: 一直自定义，包括主题等，其实就是一个CSS，有需要可以自己写或者覆盖  
slugify note url: 中文博客必须关闭，负责部分链接会异常  
path rewrite rules: 写这个笔记的时候这个功能刚出来。看了下文档，其实就是映射关系，相当于可以把A文件夹里的笔记发布到B目录下。对我来说目前用处不大。  
![Pasted image 20230328221320.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328221320.png)  
![Pasted image 20230328222032.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328222032.png)  
![Pasted image 20230328222047.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328222047.png)

### 配置模板

为了使发布博客方便，所以有必要建立一个模板，用来自动生成font matter。以下是我个人自用的模板，一般我需要发布的笔记都是直接用该模板生成。

```javascript
<%*
fileName = await tp.system.prompt("请输入笔记名", "新建笔记")
await tp.file.rename(fileName)
tp.file.cursor()

-%>
---
dg-publish: true
title: <% fileName %>
dg-created: <% tp.date.now("YYYY-MM-DDTHH:mm:ss.SSS+08:00") %>
tags: [""]

```

模板创建完成后，后续发布的笔记全部使用该模板生成，就可以自动发布了。

## 发布

笔记写完后，如果需要发布，可以点击左侧菜单的图标。则会自动显示已发布文档，有修改文档，以及未发布文档。可以一键发布。  
![Pasted image 20230328222448.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328222448.png)  
![Pasted image 20230328222507.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328222507.png)

当然有时候也只想发布一篇，可以直接ctrl + p 调出命令行，输入 digtial ,可以看到各种操作命令。  
![Pasted image 20230328222639.png](https://zytomorrow.top/img/user/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/attachments/Pasted%20image%2020230328222639.png)

### 效果展示

使用发布按钮发布后，访问博客主页即可看到。

## 自定义

digtial garden目前提供了许多的插件项，可以根据官方文档自定义设置。

### 字体美化

目前中文字体个人最喜欢的是霞骛文楷，所以博客也要用上。  
在github库 `/src/site/_includes/components/user/common/head` 路径下，新建font.njk文件（文件名随意），内容如下：

```html
<link rel="stylesheet" href="https://cdn.staticfile.org/lxgw-wenkai-screen-webfont/1.6.0/style.css" />
```

在github库 `/src/site/styles/user/` 路径下，新建font.scss文件（文件名随意），内容如下：

```scss
body {
--font-default: "LXGW WenKai Screen", sans-serif;
}
```

以上，即完成了字体美化工作。

### 增加评论系统

一个博客没有评论系统那就不完美了，所以自己也需要加上评论，我目前采用的是自建WALINE，比其他的评论系统都快，主要是可以自维护，免去审查。

在github库 `/site/_includes/components/user/common/footer/` 路径下，新建comment.njk文件（文件名随意），内容如下：

```html
<hr class="content">
<div class="content" id="waline"></div>
<link rel="stylesheet" href="https://unpkg.com/@waline/client@v2/dist/waline.css"/>
<script type="module">
import { init } from 'https://unpkg.com/@waline/client@v2/dist/waline.mjs';
init({
el: '#waline',
serverURL: 'https://comment.zytomorrow.top',
});
</script>
```

以上，即完成了评论系统的设置。

### 增加网站、文件访问统计

虽然没啥用，但是喜欢这个功能，所以还是加上了。

在github库 `/src/site/_includes/components/user/common/footer/` 路径下，新建comment.njk文件（没错，和上面同一个文件，可以合并到一起），内容如下：

```html
<hr class="content">
<div class="content" id="waline"></div>
<link rel="stylesheet" href="https://unpkg.com/@waline/client@v2/dist/waline.css"/>
    <script type="module">
        import { init } from 'https://unpkg.com/@waline/client@v2/dist/waline.mjs';
        init({
            el: '#waline',
            serverURL: 'https://comment.zytomorrow.top',
            });
</script>
<hr>
<div class="content" align="center">
<div class="content">2017–2023 ZyTomorrow</div>
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
<span id="busuanzi_container_site_pv">本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>
</div>

```

在github库 `/src/site/_includes/components/user/notes/header/` 路径下，新建busuanzi.njk文件（文件名随意），内容如下：

```html
<hr>
    <div align="center">
        <span id="busuanzi_container_page_pv">本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>
    </div>
<hr>
```

以上，即可完成统计计数。

总的来收，根据个人需要可以完成各式各样的定制，并且难度都不高。可以[参考文档](https://dg-docs.ole.dev/).