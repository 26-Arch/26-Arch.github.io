---
title: Lab1 五级流水线 CPU
parent: 实验
nav_order: 1
---

# 实验一 · Lab1 五级流水线 CPU

构建五级流水线 CPU 架构，支持指定指令集并通过 `make test-lab1` 测试。完整说明（含接线示例、波形图、常见问题）请以 **Wiki · Lab1** 为准。

[→ 打开 Wiki Lab1 完整文档](https://github.com/26-Arch/26-Arch/wiki/Lab1){: .btn }

## 目标与需要支持的指令

- **算术与逻辑**：addi, xori, ori, andi, add, sub, and, or, xor
- **扩展**：addiw, addw, subw
- **选做**：mul, div, divu, rem, remu, mulw, divw, divuw, remw, remuw

## 建议完成顺序

1. 按 **Environment** 搭建环境（Linux、Verilator、GTKWave 等），并克隆仓库、执行 `make init`。
2. 阅读 **Direction**：理解 CPU 与内存总线、Difftest 的关系及代码规范。
3. 在 `vsrc` 下实现五级流水线，并按要求连接 Difftest 三个模块（`DifftestInstrCommit`、`DifftestArchIntRegState`、`DifftestCSRState`，CSR 暂可不接）。
4. 运行 `make test-lab1`，在输出中看到 **HIT GOOD TRAP** 即表示通过。
5. 调试时可使用波形图（见下节）。

## 测试与波形图

运行 `make test-lab1`，在输出中能看到 **HIT GOOD TRAP** 即为测试通过。

需要生成波形图时使用 `make test-lab1 VOPT="--dump-wave"`，运行结束后在 `build` 目录下查看，用 gtkwave 打开。默认截取前 10^6 个时钟周期；需截取某段时使用 `VOPT="--dump-wave -b <begin> -e <end>"`。

## 提交与评分

- **内容**：包含代码与报告的 zip 压缩包；报告为 PDF。
- **打包**：在主目录下新建 `docs` 文件夹，在仓库根目录执行 `make handin`。
- **平台与截止**：Elearning，截止日期 3 月 5 日。

每个实验满分 100 分；迟交扣分，代码无法运行可能扣大部分分数。评分不参考报告长度与美观，但应清晰易读。抄袭将导致该次实验零分并可能面临更严重处罚，请独立完成。

## 常见问题摘要

| 问题 | 处理 |
|------|------|
| **No rule to make target 'emu'** | 在仓库目录下执行 `make init`。 |
| **ERROR: Unexpected CBus request modification** | 在 `iresp.data_ok` 为 1 之前不能修改 `ireq.valid` 或 `ireq.addr`；等待取指期间保持请求不变。 |
| **Settle region did not converge** | 多为组合逻辑导致信号振荡，检查相关信号；若仍报错可尝试使用低版本 Verilator（见 Wiki Lab1）。 |
| **Difftest 连接** | 原则是「指令提交（valid=1）时其影响恰好生效」。寄存器若在下一拍才写入，需将 `valid` 延迟一拍或将寄存器用“下一拍”的克隆数组接给 Difftest。详见 Wiki。 |
| **No instruction commits for 5000 cycles** | 检查波形中 `valid` 是否正常；第一条提交的指令必须是 PCINIT（`64'h8000_0000`）。 |

更多细节与 Verilator 安装、Vivado 仿真/上板说明见 [Wiki · Lab1](https://github.com/26-Arch/26-Arch/wiki/Lab1)。
