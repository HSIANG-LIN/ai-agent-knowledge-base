---
title: 04-multi-agent.md
description: 多代理人協作模式
category: Patterns
---

# Multi-Agent (多代理人) 協作模式

## 概述
當單一 Agent 的能力達到極限時，將任務拆解並分配給多個專門化的 Agent 協作，是實現複雜任務的關鍵。Multi-Agent 系統模擬了人類組織中的分工與溝通。

## 主要協作架構

### 1. Supervisor/Worker (主從模式)
這是最常見的架構，由一個「管理者 (Supervisor)」負責任務分配與流程控制。
- **Supervisor**: 接收用戶指令 $\rightarrow$ 拆解任務 $\rightarrow$ 指派給適當的 Worker $\rightarrow$ 彙整結果。
- **Worker**: 專注於執行特定任務（如：寫程式、搜尋資料、繪圖）。
- **優點**: 結構清晰，易於控制流程。

### 2. Debate (辯論模式)
透過讓兩個或多個 Agent 針對同一個問題進行辯論來減少幻覺。
- **流程**: Agent A 提出觀點 $\rightarrow$ Agent B 找出錯誤 $\rightarrow$ Agent A 辯解或修正 $\rightarrow$ 第三方（或 LLM 本身）判斷結論。
- **優點**: 極大化提升邏輯推理的嚴謹度。

### 3. Pipeline (流水線模式)
任務像生產線一樣，從一個 Agent 流向另一個。
- **流程**: `Researcher` $\rightarrow$ `Writer` $\rightarrow$ `Editor` $\rightarrow$ `Publisher`。
- **優點**: 流程高度預測，適合標準化作業流程 (SOP)。

### 4. Swarm (蜂群模式)
一種更去中心化、動態的模式，Agent 之間根據當前需求自發性地進行協作與交接。
- **特性**: 輕量級、高靈活性，適合處理高度不確定的任務。

### 5. Hermes Delegation (Hermes 委派模式)
這是 Nous Research 在設計 Hermes Agent 時採用的核心思想：
- **高效委派**: 當 Agent 發現任務超出了其能力範圍或屬於特定領域時，會主動調用 `skill` 或委派給專門的 subagent。
- **層次化架構**: 透過 `skill_view` 等機制實現工具與能力的動態載入與轉換。

## 設計挑戰
- **通訊成本 (Communication Overhead)**: Agent 之間的對話會消耗大量 Token 並增加延遲。
- **角色衝突 (Role Conflict)**: 當多個 Agent 對目標理解不一致時，會導致系統死鎖或循環。
- **狀態同步 (State Synchronization)**: 如何確保所有 Agent 都對當前的任務進度有共同的認知。
