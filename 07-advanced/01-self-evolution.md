---
title: 01-self-evolution
date: 2024-05-05
category: Advanced
tags: [self-evolution, gepa, dspy]
---

# 自我演化概念 (Self-Evolution)

超越傳統的「指令-回應」模式，讓 Agent 具備自我改進的能力。

## 1. GEPA 深度解析 (Generalized Evolutionary Prompting Architecture)
GEPA 是一個讓 Agent 在執行過程中不斷優化的架構：
- **max_metric_calls**：定義評估成功的最大嘗試次數。
- **reflection_lm**：一個專門用於「反思」執行結果的 LLM 實例。
- **validator**：根據預設規則（Schema 或邏輯）驗證執行結果的正確性。

## 2. DSPy 整合
DSPy (Declarative Self-improving Language Programs) 改變了 Prompt Engineering 的方式。
- 不再手寫 Prompt，而是透過 **Optimizer** 自動生成最適合當前任務的指令。
- 將 Agent 的行為轉化為可優化的「程式碼」。

## 3. 實戰案例：自動 Debugging Agent
1. 執行程式碼 $\rightarrow$ 2. 捕捉 Error $\rightarrow$ 3. Reflection LM 分析錯誤 $\rightarrow$ 4. 修改程式碼 $\rightarrow$ 5. 重新執行。
這形成了一個閉環的自我進化迴圈。
