# Clinical Paper Agent Prompts

> A complete set of 11 AI Agent role prompts for collaborative clinical research paper writing. Zero dependencies — works with any LLM (ChatGPT, Claude, DeepSeek, etc.).

## Overview

**11 specialized Agent prompts** for writing original clinical research papers from raw data (Excel/CSV/SPSS). Each Agent handles one stage of the paper production pipeline:

| Step | Agent | Role | Responsibility |
|------|-------|------|---------------|
| 1 | Agent1 | Data Manager | Data QC, cleaning, preprocessing |
| 2 | Agent2 | Senior Statistician | Statistical Analysis Plan (SAP) |
| 3a | Agent3a | Gold Statistician | Code execution + figures/tables |
| 3b | Agent3b | Literature Searcher | Structured literature retrieval |
| 4a | Agent4a | Introduction Writer | Introduction section |
| 4b | Agent4b | Methods+Results Writer | Methods & Results sections |
| 5 | Agent5 | Discussion Writer | Discussion section |
| 6a/b/c | Agent6a/b/c | 3 Reviewers | Clinical/Logic/Statistics review |
| 7 | Agent7 | Revision Editor | Response to reviewers + final check |

## How to Use

1. Run the pipeline sequentially (Step 0 → 7)
2. For each step, copy the corresponding Agent prompt into your LLM
3. Provide necessary context (study background, data, interim results)
4. One person should act as the **Orchestrator** (managing context flow between steps)

A detailed pipeline flowchart is available in `pipeline_overview.md`.

## File Structure

Each Agent has two prompt versions:
- `*_full.md` — Standalone version (self-contained, zero dependencies, recommended)
- `*_lite.md` — Condensed version (assumes LLM has prior knowledge of scientific writing/stats)

## Recommended Models

| Agent | Recommended Model | Rationale |
|-------|-------------------|-----------|
| Agent2 (SAP) | Gemini Flash Lite | Stable reasoning, low cost |
| Agent3a (Stats code) | Claude Haiku / GPT-4o | Strong code generation |
| Agent4a/b (Writing) | GPT-4o / Claude Sonnet | Strong English writing |
| Other Agents | DeepSeek V3 / GPT-4o mini | Sufficient, fast |

## Case Study

`examples/omg_gmg_case_study/` contains a complete trial run on an Ocular Myasthenia Gravis (OMG) → Generalized MG conversion prediction model, including:
- Study profile
- Statistical results
- Literature pool
- Final manuscript

## License

MIT
