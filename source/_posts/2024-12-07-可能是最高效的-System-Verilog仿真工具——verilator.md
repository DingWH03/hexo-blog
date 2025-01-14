---
title: 可能是最高效的(System) Verilog仿真工具——verilator
author: DingWH03
hide: false
archive: false
math: false
date: 2024-12-07 22:53:32
tags: [Verilog, System Verilog, Verilator, Linux]
categories:
- 简单记录
- 工具使用
---
## 一、介绍

Verilator 是一款免费的开源软件工具，可将 Verilog（一种硬件描述语言）转换为 C++ 或 SystemC 中的周期精确行为模型。生成的模型具有周期精确性和 2 状态性；因此，这些模型通常比更广泛使用的事件驱动模拟器提供更高的性能，后者可以在时钟周期内建模行为。Verilator 现在用于学术研究、开源项目和商业半导体开发。它是日益壮大的免费电子设计自动化 (EDA) 软件群体的一部分。

下面记录本人的安装和使用过程。

> 介绍来自[维基百科](https://en.wikipedia.org/wiki/Verilator)
>
> [官方文档](https://verilator.org/guide/latest/)
>
> [官方网站](https://www.veripool.org/verilator/)

## 二、安装

### 1. 包管理器安装

各Linux发行版包管理器不同，基于debian的发行版可使用`apt`进行安装。使用这种方式安装的verilator版本可能较低。

```bash
sudo apt install verilator
```

### 2. 克隆源码手动安装
