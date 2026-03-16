---
title: Lab2 内存访问
parent: 实验
nav_order: 2
---

# 实验二 · Lab2 内存访问

要求 CPU 支持内存读写，完整说明（包括内存总线说明、仲裁器 CBusArbiter.sv 介绍等）请以 **Wiki · Lab2** 为准。

[→ 打开 Wiki Lab2 完整文档](https://github.com/26-Arch/26-Arch/wiki/Lab-2){: .btn }

## 目标与需要支持的指令

- **访存与立即数相关指令**：
  - `ld`, `sd`
  - `lb`, `lh`, `lw`, `lbu`, `lhu`, `lwu`
  - `sb`, `sh`, `sw`
  - `lui`

Lab2 的核心目标是：在五级流水线的 Memory 阶段正确接入 `dbus`，完成对不同宽度加载/存储指令的支持，并处理好 `addr/size/strobe/data` 之间的关系。

## 建议完成顺序

1. 阅读 **实验讲解** 中关于内存总线（`ibus`/`dbus`）和 CBusArbiter 的部分，确认 `data_ok` / `addr_ok` 等握手信号的语义。
2. 在 Memory 阶段设计对 `dreq` / `dresp` 的状态机或控制逻辑，先实现最简单的对齐读写，再逐步支持字节/半字/字访问。
3. 处理好 `addr` 对齐与 `data`、`strobe` 的配合（例如写 0x1F2 一个字节时的偏移与掩码）。
4. 保持 Fetch 阶段 `ibus` 流程不被破坏，注意与 `dbus` 并发访存时由仲裁器协调。

## 测试与波形图

- 运行 `make test-lab2`，在输出中能看到 **HIT GOOD TRAP** 即为测试通过。
- 需要生成波形图时使用：

```bash
make test-lab2 VOPT="--dump-wave"
```

运行结束后在 `build` 目录下查看波形，用 gtkwave 打开。  
若需要截取特定周期范围，可使用：

```bash
make test-lab2 VOPT="--dump-wave -b <begin> -e <end>"
```

## 提交与评分

- **内容**：包含代码与报告的 zip 压缩包，报告为 PDF。
- **打包**：在主目录下新建 `docs` 文件夹，将报告命名为 `report.pdf` 放入其中，在仓库根目录执行 `make handin`，生成的 zip 直接提交。
- **提交平台与截止时间**：Elearning，截止日期以 Elearning 公告为准（Wiki 中给出推荐时间安排）。

评分方式与 Lab1 类似：以功能正确性为主，适度考虑设计合理性与报告质量，严禁抄袭。

## 常见问题摘要

- **内存写入位置不对 / 读回值错误**：检查 `addr`、`data` 与 `strobe` 是否一致，特别是未对齐地址下数据左移和字节使能的位置是否正确。
- **`data_ok` 一直不为 1 或流水线卡死**：通常是 Memory 阶段未正确维持 `dreq.valid`、`dreq.addr` 等信号，或在握手完成前多次修改请求。
- **与 Lab1 取指逻辑冲突**：确认 `ibus` 和 `dbus` 分别只在 Fetch 与 Memory 阶段驱动，不要在多个模块中重复驱动同一总线信号。

更详细的说明（包括表格定义和示例）见 [Wiki · Lab2](https://github.com/26-Arch/26-Arch/wiki/Lab-2)。

