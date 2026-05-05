---
title: 02-plan-execute.md
description: Plan-and-Execute 模式詳解
category: Patterns
---

# Plan-and-Execute 模式

## 概述
Plan-and-Execute 是一種將「規劃」與「執行」階段明確分離的 Agent 架構。相對於 ReAct 這種即時反應式的模式，Plan-and-Execute 更適合處理複雜、多步驟且目標明確的長程任務。它透過先生成一個完整的計畫，再逐一執行計畫中的步驟，來提高任務完成的穩定性。

## 架構組成
1. **Planner (規劃者)**: 接收初始目標，將其拆解為一系列有序的、可執行的步驟清單。
2. **Executor (執行者)**: 負責具體執行 Planner 產出的每一個步驟，通常會使用 ReAct 或 Tool-calling 來完成單一任務。
3. **Re-planner (動態重規劃者)**: 在每個步驟執行完畢後，根據觀察到的結果評估計畫是否仍有效。如果發現計畫中斷或環境變化，則重新生成剩餘步驟。

## 工作流程
1. **Initial Planning**: Planner 產生 `[Step 1, Step 2, ..., Step N]`。
2. **Sequential Execution**: Executor 執行 `Step 1`。
3. **Observation & Re-plan**: 
   - 如果 `Step 1` 成功且符合預期 $\rightarrow$ 執行 `Step 2`。
   - 如果 `Step 1` 失敗或結果改變了目標路徑 $\rightarrow$ Re-planner 更新計畫。
4. **Finalization**: 當所有步驟完成或目標達成時，整合結果並輸出。

## Spec-driven Implementation (規格驅動實作)
在高度工程化的 Agent 系統中，Planner 不僅是寫下步驟，更是在定義「規格」(Specification)。
- **定義輸入/輸出**: 每一步驟必須明確定義預期的 Input 與 Output 格式。
- **驗證機制**: 每一階段的結果需通過 Schema 驗證，確保下一階段的執行者能正確讀取。

## 優缺點對比

| 特性 | ReAct (Reactive) | Plan-and-Execute (Proactive) |
| :--- | :--- | :--- |
| **適用場景** | 探索性任務、即時問答 | 複雜專案、多步驟流程 |
| **容錯性** | 高 (即時修正) | 中 (需透過 Re-planner 修正) |
| **效率** | 可能陷入迴圈 | 較高 (有全局觀) |
| **複雜度** | 低 | 高 |
