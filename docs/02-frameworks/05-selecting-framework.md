---
title: 選擇框架的決策樹指南
date: 2024-05-05
tags: [framework, decision-making]
---

# 選擇框架的決策樹指南

在開發 AI Agent 時，選擇合適的框架是決定開發效率與系統能力的關鍵。本指南提供了一個決策模型，幫助你根據需求選擇最適合的工具。

## 1. 決策維度

在選擇框架前，請先評估以下四個維度：

*   **複雜度 (Complexity):** 你的 Agent 是單純的指令執行器，還是需要多個 Agent 進行複雜的協作與推理？
*   **控制力 (Control):** 你需要高度自定義底層邏輯（如特定的推理循環），還是偏好高層級的抽象（只需定義工具與目標）？
*   **生態系 (Ecosystem):** 你是否需要現成的工具集成（如 Google Search, SQL, Python REPL）？
*   **擴展性 (Scalability):** 系統是否需要處理極大量的併發請求或極長的上下文？

## 2. 決策樹指南

### 場景 A：個人助理 / 單一任務型 (Simple Agent)
*   **需求特徵：** 單一 LLM 呼叫，使用 Function Calling 完成特定任務。
*   **推薦方向：** 輕量級框架或直接使用原生 SDK。
*   **推薦工具：** OpenAI Assistants API, LangChain (Expression Language - LCEL).

### 場景 B：多角色協作 / 工作流型 (Multi-Agent / Workflow)
*   **需求特徵：** 需要多個 Agent 扮演不同角色（如：Coder, Reviewer, Manager），有明確的 SOP 或循環邏輯。
*   **推薦方向：** 基於圖形 (Graph-based) 或狀態機 (State Machine) 的框架。
*   **推薦工具：** LangGraph, CrewAI, AutoGen.

### 場景 C：複雜業務邏輯 / 高度自定義 (Enterprise / Custom Logic)
*   **需求特徵：** 需要精細控制每一個 Token 的處理流程、自定義記憶體存取機制、或整合現有的企業級工作流。
*   **推薦方向：** 低層級抽象、高度模組化的框架。
*   **推薦工具：** Semantic Kernel, 自定義開發 (Direct API integration).

## 3. 混合策略 (Hybrid Strategy)

對於大型系統，建議採用「混合架構」：
1.  **層級化設計：** 使用一個高層級的 Orchestrator (如 LangGraph) 來管理全局狀態。
2.  **模組化執行：** 將具體的任務委派給輕量級的單一 Agent 框架處理。
3.  **工具解耦：** 將工具（Tools）與 Agent 邏輯分離，透過 MCP (Model Context Protocol) 進行統一管理。

## 4. 總結對照表

| 需求類型 | 複雜度 | 控制力 | 推薦框架 |
| :--- | :--- | :--- | :--- |
| 單一功能工具 | 低 | 中 | OpenAI SDK / LangChain |
| 自動化流程/角色扮演 | 中 | 高 | CrewAI / AutoGen |
| 複雜狀態機/循環推理 | 高 | 極高 | LangGraph |
| 企業級軟體整合 | 極高 | 極高 | Semantic Kernel |
