---
title: Lab4 CSR 与后续任务
parent: 实验
nav_order: 4
---

# 实验四 · Lab4 CSR 与 Vivado 上板

Lab4 主要引入 CSR（控制状态寄存器）相关指令与寄存器，为后续异常、中断与 MMU 实验做准备。完整说明请以 **Wiki · Lab4** 为准。

[→ 打开 Wiki Lab4 完整文档](https://github.com/26-Arch/26-Arch/wiki/Lab-4){: .btn }

## 目标与需要支持的指令

- **CSR 指令**：
  - `CSRRW`, `CSRRS`, `CSRRC`
  - `CSRRWI`, `CSRRSI`, `CSRRCI`
- **需要实现的 CSR 寄存器**（均为 64 位）：
  - `mstatus`, `mtvec`, `mip`, `mie`, `mscratch`, `mcause`, `mtval`, `mepc`, `mcycle`, `mhartid`, `satp`

这些 CSR 将在 Lab5/Lab6 中用于实现异常返回、MMU 与中断处理流程。

## 建议完成顺序

1. 依据 RISC-V 手册与 Wiki，梳理各个 CSR 的基本含义、位宽与读写行为。
2. 在寄存器文件之外实现一套 CSR 存储结构，并在 Decode/Execute 阶段解析 CSR 指令，完成读写路径。
3. 确保 CSR 与普通寄存器写回路径互不干扰，必要时在 Writeback 阶段统一仲裁。
4. 为后续实验预留接口（例如异常发生时写 `mepc`/`mcause`，页表地址写入 `satp` 等），避免后面大改接口。

## 测试与波形图

当前 Lab4 的测试与上板任务在 Wiki 中「待补充」，建议在 **Verilator 仿真全部通过后** 再尝试 Vivado 仿真与上板：

- 打开 `vivado/test1/project/project_1.xpr`；
- 运行 `vsrc/add_sources.tcl` 与 `vivado/src/add_sources.tcl` 添加源文件；
- 进行仿真或生成 Bitstream，上板验证串口输出。

生成波形图的方式与前几次实验一致，可在相应 `make test-labX` 命令后添加 `VOPT="--dump-wave"`。

## 提交与评分

- **内容**：包含代码与报告的 zip 压缩包；报告为 PDF。
- **打包与提交**：参考前几个实验的 `make handin` 流程与 Elearning 提交方式。

具体评分细则与截止时间以 Wiki 与 Elearning 公告为准。

## 常见问题摘要

- **CSR 值异常**：检查读写通路是否都经由统一的 CSR 存储结构，是否存在多个地方同时驱动同一 CSR。
- **与后续实验不兼容**：实现 CSR 时即考虑 Lab5/Lab6 的需求，避免写死异常号/中断处理逻辑，保持良好扩展性。

更多设计建议请见 [Wiki · Lab4](https://github.com/26-Arch/26-Arch/wiki/Lab-4)。

