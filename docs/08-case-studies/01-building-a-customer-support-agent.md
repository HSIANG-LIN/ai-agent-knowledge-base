---
title: 實戰：客服支援 Agent
description: 從需求分析到部署上線，建構一個多平台客服 Agent 的完整流程
tags: [case-study, customer-support, tutorial, multi-platform]
date: 2026-05-05
---

# 實戰：客服支援 Agent

## 需求背景

建立一個能同時在 Telegram 和 Discord 上處理客戶問題的 AI Agent，具備知識庫查詢、工單建立、常見問題自動回覆能力。

## 架構設計

```
User (Telegram/Discord)
    │
    ▼
Hermes Gateway ──► Intent Classifier ──► FAQ (Skill)
    │                                       │
    ▼                                       ▼
Memory Provider ──► Session Context    Ticket API (Tool)
    │                                       │
    ▼                                       ▼
LLM (OpenRouter) ──► Response ──► Deliver back to user
```

## 實作步驟

1. 安裝 Hermes Agent + 設定 Telegram/Discord gateway
2. 建立 FAQ skill（SKILL.md 格式）
3. 寫一個 create_ticket 工具（MCP Server）
4. 啟用 Holographic memory provider 記錄客戶歷史
5. 設定 cron job 每日統計未結工單

## 關鍵 takeaways

- Intent classifier 放在 gateway 層可以減少不必要的 LLM 呼叫
- FAQ skill 用 YAML frontmatter 的 triggers 精準匹配常見問題
- Memory provider 讓跨 session 的客戶對話能串聯
