---
title: CrewAI
description: 基於角色扮演的多 Agent 協作框架
category: Frameworks
date: 2026-05-05
---

# CrewAI

**CrewAI** 是一個專門為「多 Agent 協作」(Multi-Agent Orchestration) 設計的框架。其核心思想是模仿現實世界中的團隊運作模式：將複雜的任務分配給具有不同角色、背景和目標的 Agent，讓它們協同工作以達成最終目標。

## 核心設計理念：Role-Based Agents

在 CrewAI 中，每一個 Agent 都被定義為一個具備特定專業領域的角色：

- **Role (角色)**：定義 Agent 的職位（例如：資深市場研究員、內容撰寫專家）。
- **Goal (目標)**：Agent 努力達成的具體任務。
- **Backstory (背景故事)**：為 Agent 提供人格化的上下文，這有助於 LLM 更準確地模擬特定角色的語氣與決策邏輯。

## 運作流程

CrewAI 的工作流程通常包含以下三個層次：

1. **Tasks (任務定義)**：明確描述需要完成的工作內容、預期結果以及對應的 Agent。
2. **Process (流程控制)**：定義 Agent 之間如何協作。例如：
    - **Sequential (順序流程)**：Agent A 完成任務後，將結果傳遞給 Agent B。
    - **Hierarchical (層級流程)**：存在一個「經理 Agent」來分配任務並監督執行結果。
3. **Tools (工具共享)**：所有的 Agent 可以共享同一組工具集，或各自擁有專屬工具，以解決不同階段的問題。

## 適用場景

- **內容創作流水線**：研究員負責蒐集資料 $\rightarrow$ 撰稿人負責寫作 $\rightarrow$ 編輯負責校對。
- **軟體開發流程**：產品經理定義需求 $\rightarrow$ 工程師撰寫代碼 $\rightarrow$ QA 進行測試。
- **市場分析**：數據分析師處理數據 $\rightarrow$ 策略專家撰寫報告。

CrewAI 的優勢在於它能有效處理「需要多種專業技能協同」的長鏈條任務，讓 Agent 的行為更具備邏輯性與結構化。
