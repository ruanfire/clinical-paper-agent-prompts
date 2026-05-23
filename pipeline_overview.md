# 临床研究生产线 — Agent & 模型总览

## 一、Agent Prompt 列表

| Agent | 角色设定 | Prompt文件 | 模型 |
|-------|---------|-----------|------|
| **小马** | 调度者/Orchestrator，负责分发任务、呈现中间结果、与哲哥交互 | —（人类） | — |
| **Agent1** | 数据管理+统计专家：质控检查、缺失分析、数据预处理 | 任务驱动（无固定角色文件） | **DeepSeek V4 Flash** |
| **Agent2** | 资深统计专家：制定SAP（研究设计、统计方法、图表计划） | 任务驱动（无固定角色文件） | **CherryIn Gemini 3.1 Flash Lite** |
| **Agent3a** | 金牌统计师：Python生成统计结果+图表，复刻R代码 | 任务驱动（无固定角色文件） | **CherryIn Claude Haiku 4.5** |
| **Agent3b** | 文献检索专家：PubMed深度检索，结构化22篇文献 | 任务驱动（无固定角色文件） | **DeepSeek V4 Flash** |
| **Agent4a** | 资深神经免疫学专家：撰写Introduction（三段式结构） | `agent_intro_role.md` | **CherryIn GPT 5.4 Nano** |
| **Agent4b** | 资深临床研究专家：撰写Methods+Results（SCI规范） | `agent_methods_results_role.md` | **CherryIn GPT 5.4 Nano** |
| **Agent5** | 资深神经免疫学专家：撰写Discussion（7段结构） | `agent_discussion_role.md` | **DeepSeek V4 Flash** |
| **Agent6a** | 审稿人A（临床设计+意义） | 任务驱动 | **DeepSeek V4 Flash** |
| **Agent6b** | 审稿人B（逻辑+证据+写作） | 任务驱动 | **DeepSeek V4 Flash** |
| **Agent6c** | 审稿人C（统计方法） | 任务驱动 | **DeepSeek V4 Flash** |
| **Agent7** | 综合修改专家：逐条回复审稿意见+全文一致性检查 | 任务驱动 | **DeepSeek V4 Flash** |

## 二、模型分配总表

| 模型 | 数量 | 分配的Agent |
|------|------|------------|
| **DeepSeek V4 Flash**（主模型） | 7 | Agent1, Agent3b, Agent5, Agent6a/b/c, Agent7 |
| **CherryIn openai/gpt-5.4-nano** | 2 | Agent4a, Agent4b |
| **CherryIn anthropic/claude-haiku-4.5** | 1 | Agent3a |
| **CherryIn google/gemini-3.1-flash-lite-preview** | 1 | Agent2 |

## 三、已保存的角色设定文件

| 文件 | 适用Agent | 保存位置 |
|------|----------|---------|
| `agent_intro_role.md` | Agent4a | D:\general\ + skill references |
| `agent_methods_results_role.md` | Agent4b | D:\general\ + skill references |
| `agent_discussion_role.md` | Agent5 | D:\general\ + skill references |

## 四、流程图

```mermaid
flowchart TB
    subgraph 哲哥["哲哥（作者）"]
        direction TB
        Z1[输入课题信息]
        Z2[反馈确认意见]
        Z3[参与审稿回复]
    end

    subgraph 小马["小马（Orchestrator）"]
        M0[Step 0: 课题初始化<br>1次交互]
        M1[Step 1: 数据质控<br>Agent1 · DS Flash<br>2轮交互]
        M2[Step 2: SAP<br>Agent2 · Gemini Flash Lite<br>1-5轮交互]
        M3[Step 3: 并行执行]
        M4[Step 4: 并行写作]
        M5[Step 5: Discussion<br>Agent5 · DS Flash<br>1-5轮交互]
        M6[Step 6: 三审并行<br>Agent6a/b/c · DS Flash]
        M7[Step 7: 回复+终稿<br>Agent7 · DS Flash<br>作者必须参与]
    end

    Z1 --> M0
    M0 --> M1
    M1 --> M2
    M2 --> M3

    subgraph Step3["Step 3: 并行执行"]
        direction LR
        A3a[Agent3a: 统计执行<br>Claude Haiku 4.5<br>Python出图+R代码]
        A3b[Agent3b: 文献检索<br>DS Flash<br>22篇结构化文献]
    end
    M3 --> A3a
    M3 --> A3b

    A3a --> M4
    A3b --> M4

    subgraph Step4["Step 4: 并行写作"]
        direction LR
        A4a[Agent4a: Introduction<br>GPT 5.4 Nano<br>三段式·500词]
        A4b[Agent4b: Methods+Results<br>GPT 5.4 Nano<br>SCI风格·800词]
    end
    M4 --> A4a
    M4 --> A4b

    A4a --> M5
    A4b --> M5

    M5 --> M6
    M6 --> M7

    M7 --> Z3

    subgraph 产出["产出物"]
        P1[00_study_profile.md]
        P2[01_data_quality.md<br>01_recode_log.md]
        P3[02_SAP.md]
        P4[03a_stat_results.md<br>03a_r_code.md<br>figures/*.png<br>tables/*.csv]
        P5[03b_literature_pool.md]
        P6[04a_intro_draft.md]
        P7[04b_methods_results_draft.md]
        P8[05_discussion_draft.md]
        P9[06_review_A/B/C.md]
        P10[07_revision_package.md<br>final_manuscript.md]
    end

    M1 --> P2
    M2 --> P3
    A3a --> P4
    A3b --> P5
    A4a --> P6
    A4b --> P7
    M5 --> P8
    M6 --> P9
    M7 --> P10
    M0 --> P1
```

## 五、试跑耗时实际统计

| Step | Agent | 模型 | 实际耗时 | 产出文件 |
|------|-------|------|---------|---------|
| 0 | 小马+哲哥 | — | ~5min | 00 |
| 1 | Agent1 | DS Flash | ~10min | 01 |
| 2 | Agent2 | Gemini Flash Lite | ~3min | 02 |
| 3a | Agent3a | Claude Haiku + 直接写 | ~20min | 03a + figures + tables |
| 3b | Agent3b | DS Flash | ~2min | 03b |
| 4a | Agent4a v1-v3 | GPT 5.4 Nano | ~15min | 04a |
| 4b | Agent4b v1-v3 | GPT 5.4 Nano | ~15min | 04b |
| 5 | Agent5 v1-v2 | DS Flash | ~4min | 05 |
| 6 | Agent6a/b/c | DS Flash | ~3min | 06 |
| 7 | Agent7 | DS Flash | ~3min | 07 + final |
| **总计** | **13个Agent** | **4种模型** | **~80min** | **23个文件** |
