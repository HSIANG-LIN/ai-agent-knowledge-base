---
title: 實用 Skills 推薦與踩坑記錄
description: 社群實用 Skills 一覽，含安裝方式、功能說明與已知踩坑
tags: [skills, community, plugins, recommendations]
date: 2026-05-05
---

# 實用 Skills 推薦與踩坑記錄

## 社群 Skill 來源

### wondelai/skills（250+ 跨平台技能）
```bash
git clone https://github.com/wondelai/skills ~/.hermes/skills/wondelai
hermes skills reload
```
涵蓋開發、DevOps、寫作、研究等常見場景。

> ⚠️ **踩坑：** 部分 skill 是針對 Claude Code 設計的，Hermes 載入後可能觸發條件不符。建議逐個檢視 SKILL.md 再啟用。

## 精選 Skills

### 開發類

| Skill | 來源 | 用途 |
|-------|------|------|
| **spec-driven-implementation** | 內建 | 依 Spec 精確實作多檔案專案，含 code review |
| **test-driven-development** | 內建 | RED-GREEN-REFACTOR 循環 |
| **full-cycle-software-engineering** | 內建 | 從需求到部署的完整專案流程 |
| **systematic-debugging** | 內建 | 4 階段根因除錯法 |
| **subagent-driven-development** | 內建 | 用 delegate_task 平行開發 |
| **codebase-inspection** | 內建 | pygount 統計專案程式碼量 |
| **execplan-skill** | [tiann/execplan-skill](https://github.com/tiann/execplan-skill) | 複雜任務執行含 checkpoint 與失敗恢復 |

### 資料科學 / 量化

| Skill | 來源 | 用途 |
|-------|------|------|
| **quant-strategy-engine** | 內建 | 模組化量化策略引擎 |
| **quant-scan-backtesting** | 內建 | 回測框架 |
| **quant-smart-report** | 內建 | 掃描結果轉報表 |
| **quant-tracking-dashboard** | 內建 | Streamlit 歷史追蹤儀表板 |
| **quant-confluence-testing-workflow** | 內建 | 量化策略 confluence 測試 |

### 維運

| Skill | 來源 | 用途 |
|-------|------|------|
| **cron-job-troubleshooting** | 內建 | 排程任務除錯 |
| **github-pr-workflow** | 內建 | PR 完整流程 |
| **github-code-review** | 內建 | PR Code Review |
| **kanban-orchestrator** | 內建 | 任務分解 + Specialist 協作 |
| **multi-project-config-sync** | 內建 | 多專案配置同步 |

### 創意 / 生產力

| Skill | 來源 | 用途 |
|-------|------|------|
| **architecture-diagram** | 內建 | 深色主題 SVG 架構圖 |
| **excalidraw** | 內建 | 手繪風格圖表 |
| **design-md** | 內建 | Google DESIGN.md Token Spec |
| **claude-design** | 內建 | 一頁式 HTML 設計原型 |
| **obsidian** | 內建 | Obsidian Vault 讀寫 |
| **notion** | 內建 | Notion API 操作 |

### 研究

| Skill | 來源 | 用途 |
|-------|------|------|
| **arxiv** | 內建 | 論文搜尋 |
| **youtube-content** | 內建 | YouTube 逐字稿摘要 |
| **blogwatcher** | 內建 | RSS/Atom feed 監控 |
| **news-aggregator-workflow** | 內建 | 台股新聞彙整 |

## Hermes 生態工具

### rtk-hermes（終端輸出壓縮）
```bash
pip install rtk-hermes
hermes plugin enable rtk-rewrite
```
壓縮 terminal 輸出約 50-90%，大幅降低 token 消耗。

> ⚠️ **踩坑：** 安裝後需重啟 gateway 才會生效。可透過 `/rtk` 指令檢查狀態。

### mission-control / Web Dashboard
Hermes v0.12.0 以上內建：
```bash
hermes web     # 啟動 Dashboard（預設 port 9119）
```
可視化管理 session、config、API keys、cron job。

### hermes-plugins（Evey 系列）
```bash
# 從 42-evey/hermes-plugins 手動安裝
```
推薦的自包含插件：
- **evey-telemetry** — JSONL 日誌記錄
- **evey-telegram-ux** — Telegram 訊息卡片格式
- **evey-goals** — 目標管理系統

> ⚠️ **踩坑：** 其他 Evey 插件（cost-guard、autonomy、status）需要完整 Evey 堆疊（Langfuse + Dashboard + MQTT），不建議單獨安裝。

### GEPA（技能自我演化）
位於 `hermes-agent-self-evolution` 套件：
- 自動優化 Skill 內容
- 使用 DSPy 進行 prompt 演化
- 透過 test questions 評估改進效果

> ⚠️ **踩坑：** 
> - DSPy 3.2.0 的參數名已從 `max_steps` 改為 `max_metric_calls`
> - Validator 的變數需傳 `raw`（含 YAML frontmatter）而非 `body`
> - 需明確設定 `reflection_lm`

## Skill 開發須知

### SKILL.md 格式
```markdown
---
name: my-skill
description: 這個技能做什麼
triggers:
  - "do X"
  - "help with Y"
---

# My Skill

## Instructions
1. 步驟一
2. 步驟二

## Examples
...
```

### 常見踩坑
1. **Triggers 太寬泛：** `"search"` 會蓋掉所有搜尋相關請求。建議用具體短語如 `"search stock"`。
2. **指令太長：** Skill 內容會注入 system prompt，太長會吃掉 token 預算。控制在 2000 chars 內。
3. **路徑寫死：** 不要寫 `~/my-project/`，用 `$PROJECT_ROOT` 或相對路徑。
4. **忘了 frontmatter：** 沒有 YAML frontmatter 的檔案不會被載入為 skill。
