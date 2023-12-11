---
{"dg-publish":true,"dg-home":true,"permalink":"/Hi , Obsidian Digital Garden/","tags":["gardenEntry"],"dgPassFrontmatter":true,"noteIcon":"","created":"2023-12-11T00:54:08.452+08:00","updated":"2023-12-11T16:55:38.664+08:00"}
---


![](https://r2.immmmm.com/2023/08/iShot_2023-08-17_21.43.16.png)

> 一个能直接把 Obsidian 文档发布建站的插件！

- GitHub 项目地址：[oleeskild/obsidian-digital-garden](https://github.com/oleeskild/obsidian-digital-garden)
- Obsidian 插件：[Digital Garden](obsidian://show-plugin?id=digitalgarden)

### 几句唠叨

几天前发现非常好用的 Obsidian 插件挂了，只能发布但不能更新和删除。经过一番搜寻找到 [@DAYU](https://anotherdayu.com/2022/4222/) 22 年底就发布文章，自己也扫过一眼，一眼，云烟，不见。

经常尝试，发现部署异常简单，简而言之就是：安装插件、填入信息、提交部署，后续更新只需要在 Obsidian 内完成。

与题图对应的 Obsidian 内容如下：

![obdg-6](https://r2.immmmm.com/2023/08/obdg-6.png)

### 站点预览

![](https://r2.immmmm.com/2023/08/odg-2.png)![](https://r2.immmmm.com/2023/08/odg-3.png)

[hermitage](https://hermitage.utsob.me/) || [gachi](https://www.gachi.cn/)

![](https://r2.immmmm.com/2023/08/odg-4.png)![](https://r2.immmmm.com/2023/08/odg-5.png)

[oldwinter](https://notes.oldwinter.top/) || [哆啦 A 梦在做梦](https://zytomorrow.top/)

### 相关教程

- 🌟 [利用 obsidian 构建个人博客](https://zytomorrow.top/%E6%8A%80%E6%9C%AF%E6%8A%98%E8%85%BE/%E5%88%A9%E7%94%A8obsidian%E6%9E%84%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
- [Obsidian 免费建站发布网页 | 基于 Digital Garden + Github + Netlify](https://anotherdayu.com/2022/4222/)
- [Obsidian Overview](https://www.gachi.cn/%E8%BD%AF%E4%BB%B6%E4%BD%BF%E7%94%A8/obsidian/obsidian/)

### 个人折腾记录

仓库地址：[https://github.com/lmm214/edui123](https://github.com/lmm214/edui123)

#### 插件安装、Github 信息填写

略，建议看一下加 🌟 的那篇。

#### 部署到 CloudFlare

官方教程 [Hosting alternatives](https://dg-docs.ole.dev/advanced/hosting-alternatives/) 提示支持：

- 自托管（尝试失败）
- Vercel： [https://vercel.com/new/clone?repository-url=https://github.com/oleeskild/digitalgarden](https://vercel.com/new/clone?repository-url=https://github.com/oleeskild/digitalgarden)
- Netlify：[https://app.netlify.com/start/deploy?repository=https://github.com/oleeskild/digitalgarden](https://app.netlify.com/start/deploy?repository=https://github.com/oleeskild/digitalgarden)
- 其它 CloudFlare、Deno、Github Pages，我采用了 CF：

打开这个仓库：[https://github.com/oleeskild/digitalgarden](https://github.com/oleeskild/digitalgarden)，点击 `Use This Template` 新建 Github 项目。

![odg-7](https://r2.immmmm.com/2023/08/odg-7.webp)

CloudFlare 中新建 `Pages` 并连接，构建命令和输出目录填写 `例如` 里的代码。

![odg-8](https://r2.immmmm.com/2023/08/odg-8.png)

至此部署完成。

### 优化踩坑记录

#### 关闭自动构建，开启部署挂钩，手动 POST 触发

记得 CF 500 次 / 个 / 月，而 Digital Garden 利用的是 Github API，所有文件修改都是一个个提交的，若不如此操作，我那昨天开新建的仓库已经有 194 commits 。

![odg-10](https://r2.immmmm.com/2023/08/odg-10.png)

![odg-11](https://r2.immmmm.com/2023/08/odg-11.png)

![odg-12](https://r2.immmmm.com/2023/08/odg-12.png)

#### 启用「霞鹜文楷」在线字体

`01-lxgw.njk`：[src/site/_includes/components/user/common/head/01-lxgw.njk](https://github.com/lmm214/edui123/blob/main/src/site/_includes/components/user/common/head/01-lxgw.njk)

下载后丢入对应文件夹内，当然，也可以直接暴力地仓库里的文件。具体说明见官方教程：[Adding custom components](https://dg-docs.ole.dev/advanced/adding-custom-components/)

#### 添加自定义样式若干

`custom-style.css`：[src/site/styles/user/custom-style.css](https://github.com/lmm214/edui123/blob/main/src/site/styles/user/custom-style.css)

#### 自定义样 favicon.svg

路径：`src/site/favicon.svg`

#### 更多折腾推荐个学习仓库

[https://github.com/uroybd/topobon/tree/main](https://github.com/uroybd/topobon/tree/main)