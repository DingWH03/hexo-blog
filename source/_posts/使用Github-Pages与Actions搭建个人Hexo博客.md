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

> 以下内容由ChatGPT生成。

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

### 如何开始使用 Hexo？

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
