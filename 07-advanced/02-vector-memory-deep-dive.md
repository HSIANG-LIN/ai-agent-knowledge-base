---
title: 02-vector-memory-deep-dive
date: 2024-05-05
category: Advanced
tags: [memory, vector, rag, knowledge-graph]
---

# 記憶系統深度解析

一個強大的 Agent 需要不同層次的記憶機制。

## 1. 記憶三劍客比較

| 特性 | 向量記憶 (Vector DB) | 全文檢索 (FTS) | 知識圖譜 (KG) |
| :--- | :--- | :--- | :--- |
| **核心原理** | Semantic Embedding | Keyword Matching | Node-Edge Relationship |
| **優點** | 理解語意相似度 | 精確匹配術語 | 強大的推理能力 |
| **缺點** | 有時會遺漏精確術語 | 無法理解語意 | 構建成本高 |

## 2. 混合搜尋 (Hybrid Search)
現代 Agent 通常結合 **Vector + FTS**。
- 先用 FTS 找關鍵字，再用 Vector 找語意，最後進行 **Rerank**（重排序）。

## 3. Trust Scoring (信任評分)
在檢索到的資訊中，並非所有資料都是正確的。
- 我們為每個記憶片段賦予一個 `trust_score`。
- 當檢索結果與已知事實衝突時，優先採用高分值的來源。
