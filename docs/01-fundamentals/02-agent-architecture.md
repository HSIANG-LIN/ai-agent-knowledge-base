---
title: Agent 架構解析
description: 從單 Agent 到 Multi-Agent 架構，深入了解 Agent 的組成要素與 Hermes Agent 的設計。
tags: [AI-Agent, Architecture, Multi-Agent]
---

# Agent 架構解析

要構建一個強大的 AI Agent，不僅僅是調用一個 LLM API，更需要設計一套完整的架構來支撐其自主運作。

## 單 Agent 架構 (Single Agent Architecture)

一個標準的單 Agent 系統通常包含以下五個核心組件：

1.  **Profile (角色設定)**：定義 Agent 的身份、專業背景、性格與行為準則（例如：「你是一位資深的 Python 工程師」）。
2.  **Instruction (指令/系統提示詞)**：明確告知 Agent 它的任務目標、限制條件以及應遵循的工作流程。
3.  **Memory (記憶體)**：
    *   **短期記憶**：當前對話的 Context，用於維持上下文的一致性。
    *   **長期記憶**：透過向量資料庫存儲的歷史知識或使用者偏好。
4.  **Tools (工具集)**：Agent 用於與外部世界互動的能力，如搜尋引擎、代碼解釋器、資料庫訪問等。
5.  **LLM Core (推理核心)**：負責邏輯推理、決策與計畫的核心模型。

## Multi-Agent 架構 (多代理人架構)

當單一 Agent 無法處理極其複雜的任務時，我們會採用 Multi-Agent 協作模式：

*   **Supervisor/Worker (監督者/執行者)**：由一個高階 Agent 負責任務拆解與分配，並監督下屬 Worker Agent 的進度。
*   **Debate (辯論模式)**：讓兩個或多個 Agent 對同一個問題提出不同觀點進行辯論，藉此提升決策的準確性與邏輯性。
*   **Pipeline (流水線模式)**：將任務拆解為一系列固定的步驟，每個步驟由專門的 Agent 完成（類似軟體開發的 CI/CD）。
*   **Swarm (集群模式)**：更動態的協作方式，Agent 之間根據需求自主尋找彼此或切換角色。

## Hermes Agent 架構深度解析

Hermes Agent 採用了高度模組化的設計，以支持高併發與複雜的任務需求：

1.  **Gateway (網關)**：所有請求的入口，負責認證、速率限制與請求分發。
2.  **Session Manager (對話管理)**：維護多個使用者的對話狀態與上下文。
3.  **Tool Dispatcher (工具分發器)**：將 LLM 的意圖精準對應到可執行的 Function 或 Skill。
4.  **Skill Engine (技能引擎)**：封裝複雜的邏輯，讓 Agent 能執行超越單一 API 的複合型任務。
5.  **Memory Provider (記憶供應商)**：統一管理短期與長期記憶的存取與檢索。

> 💡 參考架構圖請見：`assets/diagrams/agent-architecture.html` (待建立)
