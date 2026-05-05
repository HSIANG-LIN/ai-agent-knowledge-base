---
title: LangChain
description: LLM 應用開發最流行的框架
category: Frameworks
date: 2024-05-05
---

# LangChain

**LangChain** 是目前 AI 開發領域中最廣泛使用的框架之一，旨在簡化開發基於大型語言模型 (LLM) 的應用程序流程。它提供了一系列組件，讓開發者可以輕鬆地將 LLM 與外部數據源、計算能力及其他工具進行組合。

## 核心組件

LangChain 的設計哲學是將複雜的 LLM 任務拆解為可組合的模組：

- **LLMs & ChatModels**：對各種 LLM（如 OpenAI, Anthropic, Llama）的統一抽象接口。
- **Chains**：將多個組件（如 Prompt + LLM + Parser）串聯起來的邏輯鏈。
- **Agents**：讓 LLM 決定「該採取什麼行動」的智能體，透過調用工具來完成任務。
- **Tools**：Agent 可以使用的功能組件，例如 Google Search、計算機或數據庫查詢。
- **Memory**：讓 Agent 具備記憶能力，能夠在多輪對話中保持上下文。

## LangGraph：狀態機驅動的 Agent

傳統的 LangChain Chain 往往是線性的（DAG），難以處理具有循環邏輯的複雜 Agent 工作流。**LangGraph** 是 LangChain 推出的進階工具，它引入了「狀態機」(State Machine) 的概念：

- **循環 (Cycles)**：允許 Agent 在執行任務時不斷回饋與修正，形成閉環。
- **狀態管理 (State Management)**：精確控制 Agent 在每一步的內部狀態。
- **高度可控**：讓開發者能像編寫狀態機一樣，精確定義 Agent 的行為路徑。

## LangChain vs. Hermes Agent

| 特性 | LangChain | Hermes Agent |
| :--- | :--- | :--- |
| **核心目標** | 通用的 LLM 開發組件庫 | 自我改進、多平台運作的生產級 Agent |
| **開發重心** | 提供靈活的組件進行組合 | 提供完整的 Agent 運行架構與生態 |
| **部署方式** | 整合進應用程式代碼中 | 可獨立作為服務運行 (Telegram/Discord) |
| **學習曲線** | 較陡峭 (組件極多) | 中等 (聚焦於技能與流程) |

LangChain 適合需要「從零構建複雜邏輯」的場景，而 Hermes 則適合需要「快速部署具備自主能力與多平台接入」的場景。
