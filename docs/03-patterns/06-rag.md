---
title: RAG 架構與策略
date: 2026-05-05
tags: [patterns, rag]
---

# RAG 架構 (Retrieval-Augmented Generation)

RAG 是解決 LLM 「幻覺」與「知識時效性」問題的核心技術，透過檢索外部知識庫來增強模型的生成能力。

## 1. RAG 標準三階段架構

一個完整的 RAG 流程包含以下三個核心步驟：

### I. 索引階段 (Indexing)
將非結構化數據轉化為機器可檢索的形式。
*   **Document Loading:** 從 PDF, Markdown, SQL 等來源讀取數據。
*   **Chunking (分塊):** 將長文本切割成適當大小的片段。
*   **Embedding (嵌入):** 使用 Embedding 模型將文本片段轉化為高維向量。
*   **Storage:** 將向量存入向量資料庫 (Vector Database)。

### II. 檢索階段 (Retrieval)
當使用者提出問題時，尋找最相關的知識。
*   **Query Embedding:** 將使用者的問題轉化為向量。
*   **Similarity Search:** 在資料庫中計算向量相似度（如餘弦相似度）。
*   **Top-K Retrieval:** 挑選出最相關的前 $K$ 個片段。

### III. 生成階段 (Generation)
將檢索到的內容與原始問題結合。
*   **Prompt Augmentation:** 將 `[Context] + [Question]` 組合成新的 Prompt。
*   **LLM Generation:** 模型根據提供的上下文回答問題，減少幻覺。

## 2. Chunking 策略

分塊的大小與重疊度直接影響檢索品質：
*   **Fixed-size Chunking:** 固定字數，簡單但可能切斷語意。
*   **Recursive Character Chunking:** 嘗試在段落、句號處切分，保持語意完整性。
*   **Semantic Chunking:** 基於語意變動點進行切分。
*   **Overlap (重疊):** 在塊與塊之間保留一部分內容，確保語意上下文不因切分而丟失。

## 3. 檢索策略優化

*   **Hybrid Search (混合檢索):** 結合「向量檢索 (Dense)」與「關鍵字檢索 (Sparse/BM25)」。
*   **Re-ranking (重排序):** 先粗略檢索出 50 個片段，再用更強大的 Cross-Encoder 模型對這 50 個片段進行精準排序，取前 5 個。
*   **Multi-Query Retrieval:** 將一個問題改寫成多個相似問題，擴大檢索範圍。

## 4. 與 Agent Memory 的整合

RAG 不僅僅是靜態檢索，在 Agent 架構中：
*   **RAG 作為長期記憶:** 存儲大量的歷史文檔、規則或專案知識。
*   **Agentic RAG:** Agent 可以決定「是否需要檢索」、「檢索什麼」以及「檢索到的內容是否足夠」，實現主動式知識獲取。
