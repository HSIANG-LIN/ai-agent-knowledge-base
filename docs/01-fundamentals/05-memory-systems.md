---
title: 記憶系統 (Memory Systems)
description: 深入理解 Agent 的記憶分類學與 Hermes Agent 的記憶架構。
tags: [AI-Agent, Memory, Long-term Memory, RAG]
---

# 記憶系統 (Memory Systems)

對於 AI Agent 而言，記憶是維持「一致性」與「持續學習能力」的關鍵。沒有記憶的 Agent 就像是一個只有瞬間反應、卻沒有過去經驗與自我認知的生物。

## 記憶分類學 (Sternberg 模型)

參考認知心理學的 Sternberg 模型，我們可以將 Agent 的記憶分為三大類：

1.  **程序性記憶 (Procedural Memory) $\rightarrow$ 技能 (Skills)**：
    *   指 Agent 知道「如何做」某件事。
    *   在 Agent 領域，這通常體現為 Agent 對工具使用、特定工作流（Workflow）的熟練度與慣例。

2.  **情節性記憶 (Episodic Memory) $\rightarrow$ 會話 (Session)**：
    *   指對過去特定事件或對話片段的記憶。
    *   這包括了「我們剛才聊了什麼」、「上次使用者提到的偏好是什麼」。這對於維持對話上下文 (Context) 至關重要。

3.  **語意性記憶 (Semantic Memory) $\rightarrow$ 事實 (Facts)**：
    *   指對世界知識、通用事實或結構化知識的理解。
    *   這通常透過外部知識庫（如向量資料庫）來實現，讓 Agent 能檢索到其參數化知識之外的資訊。

## Hermes 記憶架構

Hermes Agent 採用了一種「分層式」的記憶管理設計，旨在平衡 Context Window 的限制與資訊檢索的深度。

### 1. Bounded Core (核心記憶)
Hermes 維護了一組精簡且高權重的核心檔案：
*   **`MEMORY.md`**：儲存當前任務的進度、已達成的目標、以及正在進行的計畫。
*   **`USER.md`**：儲存使用者的長期偏好、身份特徵與行為模式。
這類記憶會被始終放在 System Prompt 的重要位置，確保 Agent 不會忘記「我是誰」以及「我要為誰服務」。

### 2. External Providers (外部供應商)
對於海量的歷史對話與知識，Hermes 會對接外部記憶供應商：
*   **向量資料庫 (Vector DB)**：用於儲存與檢索與當前主題相關的長篇對話或文檔。
*   **RAG (Retrieval-Augmented Generation)**：當 Agent 需要特定專業知識時，會啟動檢索流程。

## Memory Provider 比較總表

| 特性 | 短期記憶 (Context Window) | 長期記憶 (Vector DB/RAG) | 核心記憶 (Bounded Core) |
| :--- | :--- | :--- | :--- |
| **存取速度** | 極快 | 中等 (需檢索) | 極快 |
| **容量限制** | 極大限制 (Token 限制) | 近乎無限 | 受到嚴格控管 |
| **主要用途** | 維持當下對話流 | 查詢歷史背景與知識 | 維持身份與核心任務 |
| **資訊密度** | 低 (原始對話) | 中 (片段/向量) | 極高 (摘要/精華) |

## 實作注意事項

開發高效的記憶系統時，必須考慮以下技術細節：

*   **Token 預算控制**：不能盲目地將所有歷史紀錄塞入 Context。必須實施「摘要化 (Summarization)」或「窗口化 (Sliding Window)」策略。
*   **相關性排序 (Relevance Scoring)**：在檢索記憶時，不應只看向量相似度，還應結合時間衰減 (Time Decay) 與任務相關性進行重排序 (Reranking)。
*   **信任評分 (Trust Scoring)**：當 Agent 檢索到可能過時或相互矛盾的資訊時，應具備判斷資訊可靠性的能力，避免錯誤資訊誤導決策。
