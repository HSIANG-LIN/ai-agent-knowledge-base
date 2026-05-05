---
title: 03-multi-agent-orchestration
date: 2024-05-05
category: Advanced
tags: [multi-agent, orchestration, coordination]
---

# 多 Agent 協調 (Orchestration)

當單一 Agent 無法處理複雜任務時，我們需要「團隊」。

## 1. 任務分解 (Task Decomposition)
將大型目標拆解為子任務。例如：
- **目標**：「分析這家公司的財報並寫報告」
- **拆解**：
  1. `Search_Agent`：搜尋財報 PDF。
  2. `Analysis_Agent`：提取關鍵財務指標。
  3. `Writer_Agent`：整合資料並撰寫報告。

## 2. 溝通模式
- **Centralized (中心化)**：由一個 Orchestrator (指揮官) 決定誰該做什麼。
- **Decentralized (去中心化)**：Agent 之間透過訊息匯流排 (Message Bus) 自主溝通。

## 3. 錯誤恢復 (Error Recovery)
當其中一個 Agent 失敗時：
- **Retry 策略**：嘗試重新執行。
- **Fallback 策略**：切換到較強大的模型（如從 GPT-4o-mini 切換到 Claude 3.5 Sonnet）。
- **Human-in-the-loop**：若多次失敗，請求人類介入。
