---
title: 使用Github Pages与Actions搭建个人Hexo博客
date: 2024-07-06 17:55:03
categories: 简单记录
tags: [Github Pages, Github Actions, Hexo, nodejs]
author: DingWH03
---

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

表1：_config.yml
| 键(Key) | 值(Val) | 描述(Description) |
| ------- | ------- | ----------------- |
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

表2：themes/fluid/_config.yml
| 键(Key) | 值(Val) | 描述(Description) |
| ------- | ------- | ----------------- |
| favicon | /img/fluid.png | 浏览器标签的图标 |
| apple_touch_icon | /img/fluid.png | 苹果设备的图标 |
| blog_title | Your blog_title | 导航栏左侧的标题 |
| menu | … | 导航栏菜单，可自行添加页面 |
| index/slogan/text | … | 显示在主页的文字 |
| about/* | … | 自定义关于页面的个人信息 |

以上只是我建议修改的配置内容，如查找进阶内容请参考[Hexo Fluid 用户手册](https://fluid-dev.github.io/hexo-fluid-docs/guide/#%E5%85%B3%E4%BA%8E%E6%8C%87%E5%8D%97)与[Hexo手册](https://hexo.io/zh-cn/docs/)自行研究。

### 增加阅读量统计

参考文章[Hexo-fluid主题设置统计博客阅读量](http://minghuijia.cn/2022/03/14/Hexo-fluid%E4%B8%BB%E9%A2%98%E8%AE%BE%E7%BD%AE%E7%BB%9F%E8%AE%A1%E5%8D%9A%E5%AE%A2%E9%98%85%E8%AF%BB%E9%87%8F/)

### 增加评论功能

参考文章[Hexo-fluid主题设置统计博客阅读量](http://minghuijia.cn/2022/03/14/Hexo-fluid%E4%B8%BB%E9%A2%98%E8%AE%BE%E7%BD%AE%E7%BB%9F%E8%AE%A1%E5%8D%9A%E5%AE%A2%E9%98%85%E8%AF%BB%E9%87%8F/)

### 显示数学公式

> 参考官方配置指南[latex-数学公式](https://fluid-dev.github.io/hexo-fluid-docs/guide/#latex-%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F)，基本配置和官方类似，但需要注意安装pandoc工具（使用mathjax），否则会报错。

#### 1. 设置主题配置

编辑主题配置文件themes/fluid/_config.yml

   ```yaml
   post:
   math:
      enable: true
      specific: false
      engine: mathjax
   ```

`specific`: 建议开启。当为 true 时，只有在文章 front-matter (opens new window)里指定 `math: true` 才会在文章页启动公式转换，以便在页面不包含公式时提高加载速度。

`engine`: 公式引擎，目前支持 `mathjax` 或 `katex`。

#### 2. 更换 Markdown 渲染器

由于 Hexo 默认的 Markdown 渲染器不支持复杂公式，所以需要更换渲染器（mathjax 可选择性更换）。

然后根据上方配置不同的 `engine`，推荐更换如下渲染器：

1. mathjax

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

并且还需安装pandoc

```bash
sudo apt-get install pandoc -y
```

2. katex

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-markdown-it --save
npm install @traptitech/markdown-it-katex --save
```

然后在站点配置中添加：

```yaml
markdown:
  plugins:
    - "@traptitech/markdown-it-katex"
```

#### 清理环境

```bash
hexo clean
```

## 添加博客页面与文章

### 添加About页面

使用`hexo new page about`即可新建一个about页面，页面默认存放在**source**文件夹中，一个页面就是一个文件夹。

### 从模板新建文章

同理，使用`hexo new post newpost`即可新建一篇文章，文章默认存放在**source/_posts**文件夹中，所有的文章都存储在**_posts**文件夹中，但可在文章同级目录建立同名文件夹以存放图片等资源文件，如下图。

{% asset_img 1.png post目录结构 %}

## 配置Github Action自动发布到Github Page

经过上面的配置操作，相信你已经成功在本地运行起来了自己的博客，但仅仅运行在本地肯定不够，我们需要将博客推送到github page中，通过username.github.io或是通过自己的域名进行访问。

### 原理

github page支持的是静态页面，而hexo编译后生成的`public`文件夹内存放的便是静态页面，因此我们只需要将`public`文件夹内的内容推送至个人github page仓库即可。

### 准备工作

首先你需要注册一个自己的github账号，并且进行了一系列的安全配置工作。

#### 创建Github仓库

我们需要创建两个仓库来完成操作，其中一个仓库名随意，我使用的名称是hexo-blog，另一个仓库名必须以`yourusername.github.io`为名字，在创建时会提示你这个仓库是特殊仓库，如下图（因为我预先创建过了，显示仓库已存在）。其中`yourusername.github.io`仓库必须设为**public**。

{% asset_img 2.png 创建仓库 %}

#### 配置git

我们需要使用git工具来将本地代码推送至github仓库，git工具在第一次使用前必须进行相关配置。参考[官方文档](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)。

#### 同步代码

为了便于操作，我使用vscode来完成git推送工作，我们首先使用git工具克隆一份空仓库，仓库地址可从Github获取，如下图。

{% asset_img 3.png 仓库URL %}

在终端使用命令`git clone your-repository-url`克隆到本地，并且将之前创建的hexo项目复制到文件夹下（要将my_blog下的所有文件以及文件夹复制到hexo-blog文件夹下，而不是复制整个my_blog文件夹），接着使用vscode打开文件夹，保持目录结构如下图。

{% asset_img 4.png 目录结构 %}

然后试试vscode的源代码管理功能吧。这里第一次点击提交是提交更改，第二次则是同步到仓库，第一次提交时必须填写消息。

{% asset_img 5.png 源代码管理 %}

同步完成后，去Github仓库看下是否同步成功吧。

#### 拷贝主题配置文件

由于我的github action脚本会自动拉取最新版本主题，因此主题的配置文件必须保存在其他地方，在部署时复制进去即可，因此我们提前将主题配置文件`themes/fluid/_config.yml`复制到`_config_theme.yml`。

#### 配置github action

##### 配置SSH密钥对

使用命令`ssh-keygen -f github-deploy-key`在当前工作目录下可生成文件`github-deploy-key`和`github-deploy-key.pub`。

复制 github-deploy-key 文件内容，在 hexo-blog 仓库 `Settings -> Secrets and variables -> Actions -> New repository secret` 页面上添加。

1. 在 Name 输入框填写 HEXO_DEPLOY_PRI。
2. 在 Value 输入框填写 github-deploy-key 文件内容。

复制 github-deploy-key.pub 文件内容，在 your.github.io 仓库 Settings -> Deploy keys -> Add deploy key 页面上添加。

1. 在 Title 输入框填写 HEXO_DEPLOY_PUB。
2. 在 Key 输入框填写 github-deploy-key.pub 文件内容。
3. 勾选 Allow write access 选项。

##### 配置GH_Token

使用hexo-deployer-git工具部署时需要github personal access token，而且这个token是不能写在配置文件中的，因此只能写在仓库环境变量中，在action部署时自动获取。点击你的GitHub头像 -> 设置 -> 开发者设置 -> Personal access tokens -> Generate new token来获取这个token,设置权限时只需要设置有关repository的读写权限即可。
复制 personal access token 内容，在 hexo-blog 仓库 `Settings -> Secrets and variables -> Actions -> New repository secret` 页面上添加。

1. 在 Name 输入框填写 GH_TOKEN。
2. 在 Value 输入框填写 personal access token 内容。

##### 添加github action

新建文件`.github/workflows/deploy.yml`，将下面的模板内容粘贴进去，然后根据自己的需要进行修改，需要修改的地方已标出。

   ```
   name: CI

   on:
   push:
      branches:
         - master

   env:
   GIT_USER: DingWH03 # 这里更改为自己的Github用户名
   GIT_EMAIL: 2521248869@qq.com # 这里更改为自己的Github绑定的邮箱
   THEME_REPO: fluid-dev/hexo-theme-fluid # 这里更改为你使用的主题的git仓库，省略github.com
   THEME_BRANCH: master # 这里更改为你使用的主题的git分支
   DEPLOY_REPO: DingWH03/dingwh03.github.io # 这里更改为你自己的仓库地址，省略github.com
   DEPLOY_BRANCH: main # 这里更改为你自己的仓库分支，一般都是main

   jobs:
   build:
      name: Build on node ${{ matrix.node_version }} and ${{ matrix.os }}
      runs-on: ubuntu-latest
      strategy:
         matrix:
         os: [ubuntu-latest]
         node_version: [16.x]

      steps:
         - name: Checkout
         uses: actions/checkout@v4

         - name: Checkout theme repo
         uses: actions/checkout@v4
         with:
            repository: ${{ env.THEME_REPO }}
            ref: ${{ env.THEME_BRANCH }}
            path: themes/fluid # 这里更换成你的主题所在路径

         - name: Checkout deploy repo
         uses: actions/checkout@v4
         with:
            repository: ${{ env.DEPLOY_REPO }}
            ref: ${{ env.DEPLOY_BRANCH }}
            path: .deploy_git

         - name: Use Node.js ${{ matrix.node_version }}
         uses: actions/setup-node@v4
         with:
            node-version: ${{ matrix.node_version }}

         - name: Configuration environment
         env:
            HEXO_DEPLOY_PRI: ${{secrets.HEXO_DEPLOY_PRI}}
            GH_TOKEN: ${{secrets.GH_TOKEN}}
         run: |
            sudo apt-get install pandoc -y # pandoc是为了启用数学公式渲染
            sudo timedatectl set-timezone "Asia/Shanghai"
            mkdir -p ~/.ssh/
            echo "$HEXO_DEPLOY_PRI" > ~/.ssh/id_rsa
            chmod 600 ~/.ssh/id_rsa
            ssh-keyscan github.com >> ~/.ssh/known_hosts
            git config --global user.name $GIT_USER
            git config --global user.email $GIT_EMAIL
            cp _config_theme.yml themes/fluid/_config.yml # 拷贝主题的配置文件
            sed -i "s|token:.*|token: ${GH_TOKEN}|" _config.yml 

         - name: Install dependencies
         run: |
            npm install

         - name: Deploy hexo
         run: |
            npm run deploy
         
         #- name: Add CNAME file # 这部分用来为github page添加自己的域名，后面会讲
         #run: |
         #   echo "blog.cxhap.top" > .deploy_git/CNAME # 改成你的域名地址
         #   cd .deploy_git
         #   git config user.name "$GIT_USER"
         #   git config user.email "$GIT_EMAIL"
         #   git add CNAME
         #   git commit -m "Add CNAME file for custom domain"
         #   git remote set-url origin git@github.com:DingWH03/dingwh03.github.io.git
         #   git push origin HEAD:main
   ```

##### 为github page添加自己的域名

> 最新更正，现在有更简单的方法，编写一个文件名为CNAME存放在`source/CNAME`，文件内容为你的域名，在部署时会直接放置到github page根目录，这样做或避免相邻两次重复部署github page。
> 参考[hexo-deployer-git_issues#87](https://github.com/hexojs/hexo-deployer-git/issues/87)。

步骤和原理都很简单，在你的域名DNS解析中添加一条CNAME解析指向你的github.io地址，然后在github.io仓库中添加一个CNAME文件，里面内容即是你的域名，在上面的脚本中已经体现出来了。

{% asset_img 6.png cloudfare %}

同步一下仓库吧，不出意外的话Github action会自动执行，并且上传到github.io中。如遇到问题欢迎与我联系。
