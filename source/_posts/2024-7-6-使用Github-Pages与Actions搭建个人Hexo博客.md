---
title: 使用Github Pages与Actions搭建个人Hexo博客
date: 2024-07-06 17:55:03
categories: Blog
tags: [Github Pages, Github Actions, Hexo]
author: DingWH03
---
使用Github Pages与Actions搭建个人Hexo博客
==========================================

## 引言

作为一名计算机专业的学生，拥有一个自己的博客网站无疑是一件非常酷的事情。然而，搭建一个网站不仅需要完成大量的工作，还需要支付一定的费用来购买或租用服务器并进行维护。那么，有没有一种更简单的方法来搭建博客呢？答案是肯定的。正如标题所示，我们可以利用 GitHub Pages 来搭建一个全前端的网站，轻松实现我们的博客需求。

## Hexo 介绍

Hexo 是一个快速、简洁且高效的静态博客框架。它使用 Markdown 或其他渲染引擎生成静态文件，并且支持丰富的插件和主题，方便用户自定义博客的外观和功能。

> 以下内容部分由ChatGPT生成。

### 为什么选择 Hexo？

1. **性能优越**：Hexo 生成的静态页面加载速度非常快，这得益于它生成的文件不需要服务器端的动态处理。
2. **易于使用**：Hexo 提供了简单易用的命令行工具，方便用户快速创建、发布和管理博客内容。
3. **灵活的插件系统**：Hexo 拥有丰富的插件库，用户可以根据需要安装插件来扩展博客的功能，如评论系统、搜索功能等。
4. **强大的主题支持**：Hexo 社区有大量精美的主题可供选择，用户可以轻松更换博客的外观，使其更具个性化。
5. **活跃的社区**：Hexo 拥有一个活跃的社区，用户可以通过社区获得帮助，分享经验和资源。

### Hexo 的基本架构

Hexo 的架构主要包括以下几个部分：

1. **Hexo 核心**：负责博客的基本功能，如文章管理、静态文件生成等。
2. **Hexo 插件**：用于扩展 Hexo 功能的模块，例如 SEO 优化、RSS 生成等。
3. **Hexo 主题**：决定博客外观和布局的模板文件，用户可以根据喜好选择和更换主题。
4. **Hexo 渲染引擎**：负责将 Markdown 等格式的源文件渲染为 HTML 静态文件。

### post和page的区别

在 Hexo 中，`page`（页面）和 `post`（文章）是两种常用的不同类型的内容：

1. **Post（文章）**：
   - **定义**：Post 是指博客中的文章或日志，通常是按时间顺序排列的内容，例如博客文章、新闻稿等。
   - **存放位置**：通常存放在 `source/_posts` 目录下（可以在配置文件中指定其他目录）。
   - **命名规则**：通常以 Markdown 或其他支持的格式书写，并以日期和标题命名，例如 `2024-07-10-my-first-post.md`。
   - **生成**：生成时会被自动排序和归档，可以通过配置自动生成目录、标签、分类等信息。

2. **Page（页面）**：
   - **定义**：Page 是指博客中的静态页面，通常不按时间顺序排列，而是作为独立的页面存在，如关于页面、联系页面等。
   - **存放位置**：通常存放在 `source` 目录下或 `source` 下的其他子目录中，如 `source/about/index.md`。
   - **命名规则**：可以是 Markdown 格式，也可以是其他支持的格式，文件名通常就是页面的路径。
   - **生成**：生成时作为站点的固定页面存在，可以通过导航链接访问。

### 如何开始使用 Hexo？

> 跳转到[本地部署Hexo博客](#本地部署Hexo博客)？

1. **安装 Node.js 和 Git**：Hexo 依赖于 Node.js 和 Git，首先需要在系统中安装这两个工具。
2. **安装 Hexo**：使用 npm（Node.js 包管理工具）安装 Hexo：
   ```bash
   npm install -g hexo-cli
   ```
3. **初始化博客**：在目标目录下初始化一个新的 Hexo 博客：
   ```bash
   hexo init my-blog
   cd my-blog
   npm install
   ```
4. **创建文章**：使用 Hexo 提供的命令创建新文章：
   ```bash
   hexo new "My First Post"
   ```
5. **生成和预览**：生成静态文件并在本地服务器中预览博客：
   ```bash
   hexo generate
   hexo server
   ```
   在浏览器中访问 `http://localhost:4000` 查看博客效果。

### 部署到 GitHub Pages

Hexo 提供了简便的部署命令，可以将生成的静态文件直接部署到 GitHub Pages：

1. **配置部署信息**：在博客根目录的 `_config.yml` 文件中配置 GitHub Pages 的仓库信息：
   ```yaml
   deploy:
     type: git
     repo: https://github.com/username/username.github.io.git
     branch: main
   ```
2. **安装部署插件**：
   ```bash
   npm install hexo-deployer-git --save
   ```
3. **执行部署命令**：
   ```bash
   hexo deploy
   ```

通过以上步骤，你就可以轻松地将 Hexo 博客部署到 GitHub Pages 上，拥有一个免费的个人博客网站。

## 本地部署Hexo博客

基本步骤可参考GPT生成部分[如何开始使用 Hexo？](#如何开始使用-Hexo？)。node.js与git的安装与配置可根据不同的操作系统在网上查找安装方法，下面将对基础的安装步骤进行补充。

### 切换主题

Hexo提供了许多免费的主题可供直接使用，用户可以个性化选择主题以达到自己想要的结果，可以去[Hexo官方主题仓库](https://hexo.io/themes/)挑选自己喜欢的主题。但需要注意的是，每款主题的配置文件可能各不相同，我使用的是**Fluid**主题，下面的配置以该主题为准，大家可自行根据配置文件注释进行配置。

#### 下载主题

前往Fluid主题官方[Github仓库](https://github.com/fluid-dev/hexo-theme-fluid)下载zip压缩包即可，直接解压至my_blog/theme目录，并将**hexo-theme-fluid**更名为**fluid**。

> 顺便在这里解释一下Hexo项目的目录结构，`node_modules`是nodejs自动生成的模块目录，`public`是编译出的静态页面地址，`scaffolds`是自动新建的页面或博客模板文件，`source`中存放页面和post的源文件（.md形式）以及部分图片，其中页面单独成一个目录，博客文件则存放在`_posts`中，关于post和页面的区别请见[post和page的区别](#post和page的区别),`theme`中存放主题。

#### 应用主题

寻找my_blog根目录下配置文件**_config.yml**，找到**theme**这一行，修改为`theme: fluid`，别忘了:后面有一个空格。修改完配置文件后自己去看看效果吧：
```bash
hexo generate
hexo server
```

### 更多配置内容

为了节约篇幅和更直观形象，下面将用表格呈现一些有用的配置内容。需要注意的是，一个Hexo项目有一个自己的配置文件`_config.yml`，此外每个主题还有一个自己的配置文件`themes/fluid/_config.yml`。
表1：
| 键(Key) | 值(Val) | 描述(Description) |
| _______ | _______ | _________________ |
| title | Your Blog Title | 浏览器tab页名称 |
| subtitle | Your Subtitle | title的副标题 |
| description | Your description | 网站的描述 | 
| author | Your Name | 文章作者名 |
| language | "zh-CN" | 网站语言 |
| timezone | '' | 时区设置，可置空 | 
| url | Your Blog url | 可设置成Github Page的地址 |
| new_post_name | :year-:month-:day-:title.md | post的模板标题 |
| post_asset_folder | true | 为每个post自动建立资源文件夹 | 
| theme | fluid | 所使用的主题名称 |

