---
title: Lab3 分支与跳转
parent: 实验
nav_order: 3
---

# 实验三 · Lab3 分支与跳转

要求 CPU 支持条件跳转与无条件跳转指令，正确处理分支指令对流水线的影响。完整说明请以 **Wiki · Lab3** 为准。

[→ 打开 Wiki Lab3 完整文档](https://github.com/26-Arch/26-Arch/wiki/Lab-3){: .btn }

## 目标与需要支持的指令

- **比较/分支与移位相关指令**：
  - `beq`, `bne`, `blt`, `bge`, `bltu`, `bgeu`
  - `slti`, `sltiu`
  - `slli`, `srli`, `srai`, `sll`, `slt`, `sltu`, `srl`, `sra`
  - `slliw`, `srliw`, `sraiw`, `sllw`, `srlw`, `sraw`
- **控制流指令**：
  - `auipc`, `jalr`, `jal`

重点是实现正确的跳转目标计算、分支条件判定，以及在流水线中处理分支带来的 flush/stall。

## 建议完成顺序

1. 在 Decode/Execute 阶段设计分支条件判定逻辑，先在单周期/简单流水线模型中验证正确性。
2. 设计 PC 更新与流水线 flush 策略：确定在哪个阶段做分支决策，以及需要 flush 哪些阶段的指令。
3. 逐步加入 `jal`/`jalr`、`auipc` 等指令的目标地址计算，保证返回地址（`ra`）写入正确。
4. 结合 Lab2 的访存部分，检查是否会与 load/use 冒险、分支冒险交互。

## 测试与波形图

- 运行 `make test-lab3`，在输出中能看到 **HIT GOOD TRAP** 即为测试通过。
- 对于实现了乘除法指令的同学，还可以运行 `make test-lab3-extra` 进行额外测试。
- 需要生成波形图时使用：

```bash
make test-lab3 VOPT="--dump-wave"
```

或指定周期范围：

```bash
make test-lab3 VOPT="--dump-wave -b <begin> -e <end>"
```

## 提交与评分

- **内容**：包含代码与报告的 zip 压缩包；报告为 PDF。
- **打包**：在主目录下新建 `docs` 文件夹，将报告命名为 `report.pdf` 放入其中，在仓库根目录执行 `make handin`，生成的 zip 直接提交。
- **提交平台与截止时间**：Elearning，截止日期以 Elearning 公告为准（Wiki 中给出了推荐时间）。

评分方式与前两次实验一致：以功能正确性与设计合理性为主，严禁抄袭。

## 常见问题摘要

- **分支目标 PC 错误**：检查立即数扩展方式（有符号/无符号）、左移位数以及与当前 PC/`auipc` 的组合是否正确。
- **流水线刷不干净**：分支成功时未完全 flush 被错误取指的阶段，导致偶发性错误；建议在波形中观察各阶段 PC 与 valid 信号。
- **`skip` 配置错误**：根据 Wiki 的说明修改 `DifftestInstrCommit` 的 `skip` 条件时，需要确保只对外设访问等特殊情况生效，不能长期为 1。

更多细节与示例请见 [Wiki · Lab3](https://github.com/26-Arch/26-Arch/wiki/Lab-3)。

