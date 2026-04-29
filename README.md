# 🚀 Agentic-SFT-Data-Synth (Demo)

> **⚠️ 说明 (Note):** > 本仓库为脱敏后的演示版本 (Demo Version)。由于深度接入内部核心业务数据，完整的多节点调度代码、RAG 知识库以及全量微调语料均受限无法开源。本仓库旨在展示**多智能体协同架构**及**核心自循环纠错逻辑**。

## 📖 项目背景 (Background)
在垂直领域大模型（LLM）微调（SFT）过程中，高质量推理数据的获取是最大瓶颈。本项目构建了一个基于大语言模型的多智能体协同数据生成管线，通过 **CoT 长链推理**与 **Self-Refine 自动化评估**，替代人工标注，实现高质量、低幻觉语料的规模化生产。

## 🧠 核心架构 (Architecture)
系统采用三步 Agent 闭环架构，支持高并发生成与自愈重写：

1. **Retriever Agent (种子构造)**: 从非结构化业务文档中提取长尾边缘场景，构建具有多样性的 Data Seed。
2. **Synthesis Agent (合成推理)**: 基于复杂思维链（CoT）策略，逆向推演问题解决路径，生成长文本对话初稿。
3. **Evaluator Agent (自循环校验)**: 核心节点。对初稿进行 5 维度打分（事实性、逻辑性、信息密度等）。当得分 $< 0.85$ 时，自动提取缺陷并打回 Synthesis Agent 重写，直至达标。

```mermaid
graph TD
    A[Raw Docs] --> B(Retriever Agent)
    B --> C(Synthesis Agent)
    C --> D{Evaluator Agent}
    D -- Score < 0.85 (Feedback) --> C
    D -- Score >= 0.85 --> E[High-Quality SFT Data]
    
