---
title: 03-reflection.md
description: 自我評估與修正模式
category: Patterns
---

# Reflection (自我反思) 模式

## 概述
Reflection 模式賦予 AI Agent 「自我審查」的能力。透過讓模型對其產出的內容、思考過程或工具執行結果進行批判性評估，可以顯著提升輸出的品質與正確性。這不僅僅是「再檢查一遍」，而是一種系統化的自我糾錯機制。

## 核心機制
Reflection 的核心在於引入一個「批判者 (Critic)」的角色，無論這個角色是另一個模型實例，還是同一個模型在不同 Prompt 下的切換。

### 1. 自我評估與修正 (Self-Correction)
模型在生成初步結果後，被要求檢查是否存在：
- **邏輯錯誤**: 推理鏈條是否斷裂？
- **事實錯誤**: 資訊是否與已知事實相符？
- **格式錯誤**: 是否符合要求的 JSON/Markdown 格式？
- **指令遵循度**: 是否漏掉了 Prompt 中的某些限制條件？

### 2. GEPA 技能演化 (Generalizable Evolutionary Pattern)
在更進階的框架下，Reflection 被用來進行「技能演化」。Agent 會記錄：
- **失敗經驗**: 「上次我用了工具 A 但失敗了，因為參數 B 格式錯誤。」
- **優化策略**: 「下次遇到此類問題，應先查詢文件再執行。」
透過將這些反思結果存入長期記憶，Agent 的能力會隨著使用次數增加而進化。

## Prompt 設計技巧

### 反思 Prompt (The Critic Prompt)
不要只說「檢查你的答案」，應給予具體的檢查維度：
> 「請扮演一名嚴格的程式碼審核員。檢查上述 Python 程式碼：
> 1. 是否包含潛在的邊界條件錯誤？
> 2. 是否符合 PEP8 規範？
> 3. 是否有資源釋放（如 close file）的缺失？
> 請列出所有錯誤，並給出改進建議。」

### 迭代結構 (Iterative Structure)
1. **Generate**: 模型生成初稿。
2. **Critique**: 模型找出問題點。
3. **Refine**: 模型根據問題點重寫。

## 常見陷阱
- **過度批評 (Over-correction)**: 模型可能在明明正確的內容上「找碴」，導致結果變得冗長或變得不正確。
- **同質化偏見 (Self-Confirmation Bias)**: 如果使用同一個 Prompt 進行反思，模型往往會傾向於認同自己原本的錯誤答案。
  - *解決方案*: 使用不同的 System Prompt 或更強大的模型作為 Critic。
