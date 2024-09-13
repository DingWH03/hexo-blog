---
title: 使用markdown制作ppt演示并嵌入到博客中
author: DingWH03
hide: false
archive: false
math: false
date: 2024-09-08 13:03:49
tags: [Slidev, Web PPT, Hexo, nodejs]
categories: Blog
---

在学习rcore课程与观看开源操作系统夏令营课程时，偶然发现老师使用markdown文件编译出的web ppt，于是吸引了我的注意，遂自行进行尝试，以本文记录slidev的使用方法以及嵌入博客的方法，供本人使用时快速查阅参考。

另外，还有一个更简洁的项目名为[nodeppt](https://github.com/rrrene/nodePPT)，但已许久未进行更新，并且可能不兼容nodejs v22，遂并未使用。网上有诸多博客记录如何将nodeppt嵌入hexo博客中，参考那些博客我也顺利将slidev嵌入到hexo中。

> 本文参考自官方[快速上手指南](https://cn.sli.dev/guide/)，仅记录本人常用部分内容，需要获取更多信息或进阶操作请自行阅读手册。

## 官方[demo](../../../../slidev/demo/index.html)

<iframe src="../../../../slidev/demo/index.html" width="100%" height="500" name="topFrame" scrolling="yes" noresize="noresize" frameborder="0" id="topFrame"></iframe>

## 安装slidev命令行工具

slidev由node.js提供，在安装slidev之前需要先保证node.js安装配置正确，并且版本号大于v18。并且出于网速考虑，建议在安装之前确保已经设置可用的软件源。

```shell
sudo npm install @slidev/cli -g
```

## 创建slidev项目

```shell
npm init slidev@latest # 使用npm
```

在创建项目时会询问是否需要自动执行`npm install`与`npm run dev`，根据需要进行选择。若选择否则需要自行根据提示进入到目录内执行安装和服务命令。

> 官方还提供了使用StackBlitz在浏览器中在线创建幻灯片的形式[sli.dev/new](https://sli.dev/new)，但我没进行尝试，看起来可以省去安装环境的烦恼。

## slidev常用方法

```shell
slidev # 启动开发服务器
slidev export # 将幻灯片导出为 PDF、PPTX 或 PNG 文件
slidev build # 将幻灯片构建为静态网页
slidev format # 将幻灯片格式化
slidev --help # 显示帮助信息
```

详见[导出为其他文件](#导出为其他文件)、[构建静态网页](#构建静态网页)、[格式化幻灯片](#格式化幻灯片)部分。

## ppt演示

### 界面

{% asset_img ui.png ui %}

如上图所示，将鼠标移至界面左下角可见悬浮的工具栏，从左往右依次是`全屏`、`上一页`、`下一页`、`总览`、夜(日)间模式、`绘制工具`、`演讲者视角`、`信息`、`设置`、`幻灯片页码`。

### 快捷键

部分常用快捷键如下，slidev演示还支持[自定义快捷键](https://cn.sli.dev/custom/config-shortcuts)，可根据官方手册进行尝试。

| 快捷键 | 动作                     |
|--------|--------------------------|
| f      | 切换全屏                 |
| right / space | 下一动画或幻灯片       |
| left   | 上一动画或幻灯片         |
| up     | 上一张幻灯片             |
| down   | 下一张幻灯片             |
| o      | 切换幻灯片总览           |
| d      | 切换暗黑模式             |
| g      | 显示“前往...页”            |

## 导出为其他文件

### 准备工作

导出为 PDF、PPTX 或 PNG 依赖于 [Playwright](https://playwright.dev/) 来渲染幻灯片。因此，你需要在你的项目中安装 [`playwright-chromium`](https://npmjs.com/package/playwright-chromium)：

```shell
npm i -D playwright-chromium
```

### 支持的格式

#### PDF

```shell
slidev export
```

#### PPTX

```shell
slidev export --format pptx
```

需要注意的是，PPTX 文件中的所有幻灯片都会被导出为图片，因此文本不可选择。演讲者备注将以每张幻灯片为单位传递到 PPTX 文件中。

在此模式下，默认启用了 `--with-clicks` 选项。要禁用它，请传递 `--with-clicks false`。

> `--with-clicks` 选项将幻灯片的多个步骤导出为多个页面，包含点击动画，在进行其他类型导出时可以手动加上。

#### PNGs 或 Markdown

当传递 `--format png` 选项时，Slidev 会为每张幻灯片导出 PNG 图像而不是 PDF：

```shell
slidev export --format png
```

你也可以使用 `--format md` 选项将幻灯片导出为 Markdown 文件：

```shell
slidev export --format md
```

#### PDF 大纲

```shell
slidev export --with-toc
```

更多选项见[手册-导出幻灯片](https://cn.sli.dev/guide/exporting)。

## 格式化幻灯片

```shell
slidev format [entry]
```

格式化 markdown 文件。请注意，这不会格式化幻灯片的内容，只会格式化 markdown 文件的组织结构。

- `[entry]` (`string`, 默认值: `slides.md`): 幻灯片的 markdown 文件路径

## 构建静态网页

你可以通过以下命令将幻灯片构建为静态的 [单页应用 (SPA)](https://developer.mozilla.org/en-US/docs/Glossary/SPA)：

```shell
slidev build
```

默认情况下，生成的文件会被放在 `dist` 文件夹中。你可以通过运行 `npx vite preview` 或任何其他静态服务器来测试你的幻灯片的构建版本。

> 运行`npx vite preview`命令时需要在当前工作路径下存在`dist`文件夹，而不是在`dist`文件夹内。

### 基础路径

若要将你的幻灯片部署在子目录下，你需要传递 `--base` 选项。`--base` 路径**必须以斜杠 `/` 开头和结尾**。例如：

```shell
slidev build --base /slidev/demo/
```

基础路径很重要，若是将ppt部署在hexo内，必须将基础路径修改成对应于hexo的`source`路径。具体说明详见[嵌入到博客中](#嵌入到博客中)。

### 输出目录

你可以通过 `--out` 选项更改输出目录：

```shell
slidev build --out my-build-folder
```

## 嵌入到博客中

将slidev生成的静态页面嵌入hexo中，亲测可以正常放映以及可以进入全屏模式放映。

### 配置hexo编译跳过目录

在Hexo博客里想调用或者链接slidev生成的html，需要hexo设置`skip_render`, 指定不进行渲染的文件或文件夹，例如在`source`目录下新建`slidev`来存放slidev生成的html，则需要在根目录下的`_config.yml`文件添加：

```yaml
skip_render: 
  - slidev/**
```

### 构建slidev静态页面

例如我将把页面放在博客源码的source/slidev/demo路径下，执行命令`slidev build --base /slidev/demo/`即可。

### 放入静态页面

将编译出的文件夹`dist`直接放入`source/slidev/`目录下，并改名为`demo`。

### 在文章中引用

首先需要知道ppt相较于当前文章路径的相对路径，如下图，在我的博客编译后的`public`路径下，需要向上4层才能回到根路径，`public`路径便视为根目录，因此该ppt对本文章的相对路径则为`../../../../slidev/demo/index.html`，因此在文章内访问该相对地址可以直接访问静态资源。

{% asset_img 路径.png 路径 %}

### 嵌入到文章中

通过插入`iframe`标签实现：

```html
<iframe src="../../../../slidev/demo/index.html" width="100%" height="500" name="topFrame" scrolling="yes" noresize="noresize" frameborder="0" id="topFrame"></iframe>
```

## Slidev Markdown语法

详见[手册-语法](https://cn.sli.dev/guide/syntax)。
