---
title: 規劃與推理模式
description: 深入探討 Agent 如何進行思考、任務分解與動態重規劃。
tags: [AI-Agent, Planning, Reasoning, ReAct]
---

# 規劃與推理模式

AI Agent 的核心價值在於其「思考」的能力。面對複雜問題時，Agent 不能僅靠直覺回答，必須具備系統性的規劃與推理能力。

## ReAct 模式：思考、行動、觀察

**ReAct (Reason + Act)** 是目前最主流的 Agent 運作模式。它建立了一個循環，讓 Agent 在每一步行動前都進行邏輯思考。

其標準循環流程如下：

1.  **Thought (思考)**：Agent 分析當前的目標與已知的資訊，決定下一步需要做什麼。
2.  **Action (行動)**：根據思考結果，選擇一個特定的工具並提供參數。
3.  **Observation (觀察)**：獲取工具執行的結果（例如 API 回傳的資料或錯誤訊息），並將其納入下一次的思考。

透過這種「思考 $\rightarrow$ 行動 $\rightarrow$ 觀察」的循環，Agent 能夠在與環境互動的過程中不斷修正自己的路徑。

## Plan-Ahead：任務分解與動態重規劃

對於大型任務，Agent 需要具備「預見性」：

*   **任務分解 (Task Decomposition)**：將模糊的大目標拆解成一系列具體的、有依賴關係的子任務 (Sub-tasks)。
*   **子任務依賴圖 (Dependency Graph)**：識別哪些任務可以並行處理，哪些必須按順序執行。
*   **動態重規劃 (Dynamic Replanning)**：當執行過程中發現環境變化或預期結果不符時，Agent 必須具備修改原定計劃的能力，而非死板地執行舊指令。

## Tree-of-Thoughts (ToT)：多路徑探索

**Tree-of-Thoughts** 是一種更進階的推理方法。不同於單線性的思考，ToT 允許 Agent 在決策點進行「分叉」：

1.  **分支探索**：針對一個問題，生成多個潛在的解決方案（Thoughts）。
2.  **自我評估**：對每一條路徑進行評估（例如：成功率、成本、可行性）。
3.  **回溯 (Backtracking)**：如果發現某條路徑走不通，Agent 會回溯到之前的決策點，選擇另一條更有希望的路徑。

## 實作細節考量

在實際開發 Agent 規劃模組時，必須注意：

*   **Plan 的序列化格式**：如何將計畫以 JSON 或特定的 Markdown 結構儲存，以便 LLM 能夠穩定地理解與更新。
*   **狀態恢復 (State Recovery)**：當 Agent 在執行複雜計畫的中途崩潰或中斷時，如何利用已儲存的狀態 (State) 進行恢復，避免從頭開始浪費 Token。
