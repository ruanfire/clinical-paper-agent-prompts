# Clinical Paper Pipeline — Agent & Model Overview

## 1. Agent Prompts (v2.2.0 — 14 Agents)

| Agent | Role | Prompt File | Recommended Model |
|-------|------|------------|------------------|
| **Orchestrator** | Human coordinator: distributes tasks, presents interim results, interacts with author | — (Human) | — |
| **Agent1** | Data manager: QC, quality rating (A-F), fraud detection, preprocessing | `agent1_data_role_full.md` | DeepSeek V4 Flash / GPT-4o mini |
| **Agent2** | Senior statistician: SAP (study design, methods, figure plan) | `agent2_sap_role_full.md` | Gemini Flash Lite |
| **Agent3a** | Gold statistician: Python/R code, figures & tables | `agent3a_stat_role_full.md` | Claude Haiku / GPT-4o |
| **Agent3b** | Literature searcher: PubMed search, structured extraction | `agent3b_literature_role_full.md` | DeepSeek V4 Flash |
| **Agent4a** | Introduction writer: 3-paragraph structure (burden→gap→purpose) | `agent4a_intro_role_full.md` | Claude Sonnet 4.6 |
| **Agent4b** | Methods+Results writer: SCI-standard reporting | `agent4b_methods_role_full.md` | Qwen 3.5 Plus |
| **Agent5** | Discussion writer: 7-segment structure + 3-4 limitations with mitigations | `agent5_discussion_role_full.md` | DeepSeek V4 Pro |
| **Agent2** | Senior statistician: SAP (study design, methods, figure plan) | `agent2_sap_role_full.md` | Claude Haiku 4.5 |
| **Agent3a** | Gold statistician: Python/R code, figures & tables | `agent3a_stat_role_full.md` | Claude Haiku 4.5 |
| **Agent6b** | Reviewer B (logic + evidence) | `agent6b_reviewer_logic_role_full.md` | DeepSeek V4 Flash |
| **Agent6c** | Reviewer C (statistics) | `agent6c_reviewer_stats_role_full.md` | DeepSeek V4 Flash |
| **Agent7** | Revision editor: point-by-point response, consistency check | `agent7_revision_role_full.md` | DeepSeek V4 Flash |
| **Agent9** | Cover Letter writer: submission letter + suggested reviewers | `agent_coverletter_role_full.md` | DeepSeek V4 Flash |
| **Agent10** | Graphical Abstract: visual abstract text description | `agent_graphical_abstract_role_full.md` | DeepSeek V4 Flash |

## 2. Flowchart

```
Author ──→ Orchestrator ──→ 14 Agents ──→ 25+ output files
                              │
    Step 0: Topic Initiation ◄──────────────── Author input
         │
    Step 1: Data QC + Quality Rating (Agent1) ──→ A/B/C/D/F grade
         │     ⚠️ F-grade → HALT pipeline
         │               └→ data quality report + rating
    Step 2: SAP (Agent2)
         │               └→ SAP document
    Step 3: ┌──── Parallel ────┐
            │                  │
      Agent3a (Stats Code)    Agent3b (Literature)
      Python+figures+R-code   Structured 22 refs
            │                  │
            └──────┬───────────┘
    Step 4: ┌──── Parallel ────┐
            │                  │
      Agent4a (Introduction)  Agent4b (Methods+Results)
            │                  │
            └──────┬───────────┘
    Step 5: Discussion (Agent5) ──→ 3-4 limitations with mitigations
         │               └→ discussion draft
    Step 5.5: Abstract (Agent8) ──→ Title + Abstract + Keywords
         │
    Step 6: ┌─── 3 Parallel Reviews ───┐
            │       │         │        │
       Agent6a   Agent6b   Agent6c
        Clinical   Logic     Stats
            │       │         │
            └─── 3 review reports ───┘
    Step 7: Revision + Final (Agent7)
         │  └→ revision package + final manuscript
         │
    Step 7b: Cover Letter (Agent9) + Graphical Abstract (Agent10)
         │
         Author approval ──→ Complete
```

## 3. Pipeline Workflow

| Step | Agent | Description | Output |
|------|-------|-------------|--------|
| 0 | Orchestrator + Author | Define study topic, PICOS, target journal | `00_study_profile.md` |
| 1 | Agent1 | Data QC, quality rating (A-F), fraud detection, variable processing | `01_data_quality.md`, `01_recode_log.md` |
| 2 | Agent2 | Statistical Analysis Plan | `02_SAP.md` |
| 3a | Agent3a | Execute stats, generate figures + R code | `03a_results.md`, `03a_R_code.md`, figures, tables |
| 3b | Agent3b | Literature search, structured extraction | `03b_literature_pool.md` |
| 4a | Agent4a | Write Introduction (3-paragraph) | `04a_intro_draft.md` |
| 4b | Agent4b | Write Methods + Results | `04b_methods_results_draft.md` |
| 5 | Agent5 | Write Discussion (3-4 limitations with mitigations) | `05_discussion_draft.md` |
| 5.5 | Agent8 | Write Title + Abstract + Keywords | `05_abstract_draft.md` |
| 6 | Agent6a/b/c | 3 parallel peer reviews | `06_review_A/B/C.md` |
| 7 | Agent7 | Point-by-point response + consistency check | `07_revision_package.md`, `07_checklist.md` |
| 7b | Agent9 + Agent10 | Cover Letter + Graphical Abstract | `07_coverletter.md`, `07_graphical_abstract.md` |
| Final | — | Full manuscript | `final_manuscript.md` |

## 4. Data Quality Rating System

Agent1 must output a data quality grade before proceeding:

| Grade | Label | Criteria | Action |
|-------|-------|----------|--------|
| A | Excellent | Missing <5%, no outliers, no contradictions | Proceed |
| B | Good | Missing 5-15%, minor outliers | Proceed after cleaning |
| C | Marginal | Missing 15-30%, moderate issues | Discuss with author first |
| D | Poor | Missing >30% key variables, severe issues | ⛔ Pause, verify data source |
| F | Fraudulent | Benford violation, uniform p-values, impossible data | 🚫 **HALT** — replace dataset |

## 5. Interaction Pattern

Clinical research is NOT a fully automated pipeline. Each step requires Orchestrator-Author interaction:

```
Agent output → Orchestrator summarizes → Presents to Author → Author gives feedback → Orchestrator passes to next Agent
```

- **Step 0**: Fixed 1 round
- **Steps 1-7**: Minimum 1 round, max 5 rounds per interaction node
- **Step 1 (F-grade)**: HALT, no further steps allowed
- **Step 7**: Author MUST participate in reviewer response decisions

## 6. Trial Run Metrics (OMG→GMG Case)

| Step | Agent | Model | Actual Time | Output Files |
|------|-------|-------|-------------|--------------|
| 0 | Orchestrator | — | ~5min | 1 |
| 1 | Agent1 | DS Flash | ~10min | 2 |
| 2 | Agent2 | Gemini Flash Lite | ~3min | 1 |
| 3a | Agent3a | Custom script | ~20min | 4 figures + 3 tables |
| 3b | Agent3b | DS Flash | ~2min | 1 |
| 4a | Agent4a (v1-v3) | GPT 5.4 Nano | ~15min | 1 |
| 4b | Agent4b (v1-v3) | GPT 5.4 Nano | ~15min | 1 |
| 5 | Agent5 (v1-v2) | DS Flash | ~4min | 1 |
| 6 | Agent6a/b/c | DS Flash | ~3min | 3 |
| 7 | Agent7 | DS Flash | ~3min | 2 |
| **Total** | **14 Agents** | **4 model types** | **~80min** | **23 files** |
