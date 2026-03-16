---
title: Lab5 MMU 与 ECALL
parent: 实验
nav_order: 5
---

# 实验五 · Lab5 MMU 与 ECALL

Lab5 主要实现异常返回指令与内存管理单元（MMU），为支持操作系统级的地址空间管理奠定基础。完整说明请以 **Wiki · Lab5** 为准。

[→ 打开 Wiki Lab5 完整文档](https://github.com/26-Arch/26-Arch/wiki/Lab-5){: .btn }

## 目标与需要支持的功能

- **指令**：
  - `MRET`：从机器态异常返回。
  - `ECALL`：环境调用异常（系统调用入口）。
- **MMU**：
  - 实现 Sv39 页表，完成虚拟地址到物理地址的转换。

Wiki 特别提示：设计 `ECALL` 时要一并考虑 Lab6 的异常处理流程，不要做成与后续实验不兼容的特例。

## 建议完成顺序

1. 在已有 CSR 基础上，梳理 `mepc`、`mcause`、`mtval`、`mtvec` 等在异常/返回中的读写时机。
2. 先在物理地址下实现 `ECALL` 触发异常并跳转到异常入口、`MRET` 正确返回的最小闭环。
3. 设计 Sv39 地址转换流程（页表遍历、TLB 设计可按课程要求自行取舍），在 Memory 阶段或访存接口处嵌入转换逻辑。
4. 将 `ECALL`、页错误等异常与 Lab6 的中断框架预留好接口，避免后续大规模重构。

## 测试与波形图

Lab5 的具体测试命令与用例见 Wiki。一般流程：

- 使用 `make test-lab5` 运行测试；输出中出现 **HIT GOOD TRAP** 表示通过。
- 需要波形图时使用：

```bash
make test-lab5 VOPT="--dump-wave"
```

或指定区间：

```bash
make test-lab5 VOPT="--dump-wave -b <begin> -e <end>"
```

## 提交与评分

- **内容**：包含代码与报告的 zip 压缩包；报告为 PDF。
- **打包与提交**：参考前几次实验的 `make handin` 流程与 Elearning 提交方式。

评分重点在于：地址转换与异常返回逻辑的正确性、设计合理性以及与后续 Lab6 的兼容性。

## 常见问题摘要

- **ECALL 行为异常**：未正确写入 `mepc`/`mcause`/`mtval`，或返回路径上的 `MRET` 未恢复正确的特权级与 PC。
- **MMU 转换错误或死循环**：页表遍历条件、权限检查或终止条件实现不当，建议结合波形和日志仔细排查。

更多细节请见 [Wiki · Lab5](https://github.com/26-Arch/26-Arch/wiki/Lab-5)。

