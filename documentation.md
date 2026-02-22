---
title: 文档说明
nav_order: 3
---

# 文档说明

课程材料分为两部分：**代码仓库**（26-Arch）和 **Wiki**（说明文档）。请先克隆代码仓库并初始化子模块，再做实验。

## 各文档用途

| 文档 | 用途 |
|------|------|
| **Wiki · Home** | 课程首页：答疑方式、参考文档链接等。 |
| **Wiki · Environment** | 实验环境搭建与介绍：Linux/WSL、Verilator、GTKWave、Vivado 的安装与配置，以及 VS Code 连接、克隆仓库与更新步骤。做实验前请先完成环境搭建。 |
| **Wiki · Direction** | 整体流程与代码编写指南：CPU 与 Difftest 的关系、代码规范、内存总线接口、如何接线与测试、常见问题等。建议在写代码前通读。 |
| **Wiki · Lab1** | 实验一详细说明：目标指令、测试方法、Difftest 接线、波形图生成、提交方式、评分标准、常见问题等。做 Lab1 时以该页为准。 |

## 如何获取与更新代码

在选定目录下执行：

```bash
git clone https://github.com/26-Arch/26-Arch.git
cd 26-Arch
git submodule update --init --recursive   # 初始化 difftest 子模块
```

每次助教发布新内容后，请先 `commit` 本地改动，再拉取并合并：

```bash
git fetch --all
git merge origin/main
git submodule update
```

{: .note }
代码需在 `vsrc` 目录下编写，核心在 `vsrc/src/core.sv`。不要修改 `vsrc` 以外的文件，除非你清楚自己在做什么。
