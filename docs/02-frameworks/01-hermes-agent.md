---
title: Hermes Agent
description: Nous Research 開發的自我改進 AI Agent 框架
category: Frameworks
date: 2024-05-05
---

# Hermes Agent

Hermes Agent 是由 **Nous Research** 開發的高性能、自我改進型 AI Agent 框架。它不僅僅是一個聊天機器人，而是一個能夠在多個平台運行、具備程序性記憶並能透過技能學習不斷進化的智能體。

## 核心特性

- **自我改進 (Self-Improvement)**：透過技能學習機制，Agent 可以不斷優化自身的執行能力。
- **多平台通訊**：原生支援 Telegram, Discord, Slack 以及 CLI，使其能輕易整合進現有的通訊工作流。
- **程序性記憶 (Procedural Memory)**：不只記得事實，還能記住「如何做某件事」的操作流程。
- **技能學習系統 (Skill Learning)**：透過 `SKILL.md` 定義新能力，並利用 GEPA (Generative Evolutionary Process Architecture) 進行技能演化。
- **MCP 整合**：全面支援 Model Context Protocol (MCP)，可輕鬆擴展工具生態。
- **排程能力**：內建 cron 功能，支援定時任務執行。

## 架構設計

Hermes Agent 採用模組化設計，確保了高度的可擴展性：

1. **Gateway**：處理各種平台的輸入（如 Telegram 訊息）。
2. **Session Manager**：管理使用者對話上下文與狀態。
3. **Tool Dispatcher**：根據需求分發工具調用請求。
4. **Skill Engine**：核心邏輯，負責技能匹配與執行。
5. **Memory Provider**：負責長期與短期記憶的存儲與檢索。

## 安裝方式

你可以透過以下兩種方式安裝 Hermes Agent：

### 方法 A：從原始碼安裝 (推薦開發者使用)
```bash
git clone https://github.com/nousresearch/hermes-agent.git
cd hermes-agent
pip install -e .
```

### 方法 B：透過 pip 直接安裝
```bash
pip install hermes-agent
```

## 生態工具

- **rtk-hermes**：專為終端設計的壓縮工具，優化 CLI 交互。
- **mission-control**：提供直觀的 Web Dashboard，用於監控 Agent 狀態。
- **hermes-plugins**：Evey 系列插件，擴展 Agent 的特定領域能力。
