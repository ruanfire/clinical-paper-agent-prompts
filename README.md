# Clinical Paper Agent Prompts

> 一套用于临床研究论文全流程协作的AI Agent角色设定prompt。零依赖，任何LLM（ChatGPT/Claude/DeepSeek等）均可使用。

## 概述

基于临床数据（Excel/CSV/SPSS）撰写原创临床研究论文的**11个Agent角色prompt**。每个Agent负责论文生产流程中的一个专业环节：

| Step | Agent | 角色 | 负责内容 |
|------|-------|------|---------|
| 1 | Agent1 | 数据管理专家 | 数据质控、清洗、预处理 |
| 2 | Agent2 | 资深统计学家 | 统计分析计划(SAP)撰写 |
| 3a | Agent3a | 金牌统计师 | 统计代码执行+图表生成 |
| 3b | Agent3b | 文献检索专家 | 结构化文献检索 |
| 4a | Agent4a | Introduction写手 | 引言撰写 |
| 4b | Agent4b | Methods+Results写手 | 方法+结果撰写 |
| 5 | Agent5 | Discussion写手 | 讨论撰写 |
| 6a/b/c | Agent6a/b/c | 3位审稿人 | 临床/逻辑/统计三审 |
| 7 | Agent7 | 综合修改专家 | 审稿回复+终审 |

## 使用方式

1. 选择对应环节的prompt文件
2. 将完整prompt粘贴到你的LLM对话中
3. 提供必要的上下文（研究背景、数据等）
4. 建议由一人（Orchestrator）协调全流程

## 文件说明

每个Agent有两个版本：
- `*_lite.md` — 精简版（适用于已有科研知识的LLM）
- `*_full.md` — 完整版（自包含，零依赖，推荐首次使用）

## 模型推荐

| Agent | 推荐模型 | 理由 |
|-------|---------|------|
| Agent2 SAP | Gemini Flash Lite | 推理稳定，成本低 |
| Agent3a 统计执行 | Claude Haiku / GPT-4o | 代码生成质量高 |
| Agent4a/b 写作 | GPT-4o / Claude Sonnet | 英文写作能力强 |
| 其余Agent | DeepSeek V3 / GPT-4o mini | 够用，速度快 |

## 案例

`examples/omg_gmg_case_study/` 目录包含一次完整的试跑案例（眼肌型重症肌无力→全身型转化预测模型）。

## License

MIT
