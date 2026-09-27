# Llama-3-8B-Instruct HBM Fault Injection

本项目研究 Llama-3-8B-Instruct 在 HBM 1→0 非对称位翻转故障下的模型容错能力，重点评估 FP16 与 Quanto INT8 模型在故障注入和保护机制下的准确率变化。

## 实验模型与配置

实验覆盖以下模型表示和保护方式：

- FP16：高精度基线
- Quanto INT8：无保护的量化基线
- Quanto INT8 + BER=0.003：对有效权重 bit 注入约 0.3% 的 1→0 故障
- Quanto INT8 + SpECC：使用 SpECC 冗余编码保护的 INT8 模型
- Quanto INT8 + SRLR：使用 SRLR 冗余保护机制的 INT8 模型

## 故障模型

故障模型模拟 HBM 电容泄漏造成的 1→0 bit flip。每轮实验从干净模型重新加载，在指定权重范围内按照固定随机种子、均匀且无放回地选择初始值为 1 的有效 bit，并将其翻转为 0。实验统一采用约 0.3% BER，同时记录实际翻转数量和实际 BER。

## 评测数据集

- **MMLU**：通用知识与推理能力
- **MathQA**：数学问题求解能力
- **HumanEval**：代码生成能力

## 保护机制评估

项目比较无保护 INT8、SpECC 保护和 SRLR 保护在 HBM 持久性故障下的恢复能力。每个实验脚本记录模型配置、量化方式、故障注入范围、随机种子、GPU、翻转数量、BER 和最终准确率，便于复现实验并进行横向比较。

## 数据与复现说明

模型文件、完整运行日志、原始 mask、缓存和大规模结果文件不上传到 GitHub。仓库仅保存可复现实验所需的程序代码、配置文件、说明文档和结果汇总。详细实验设置和结果说明见 `github_release/docs/EXPERIMENTS.md` 与 `github_release/results_summary/`。
