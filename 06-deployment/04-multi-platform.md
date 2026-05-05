---
title: 04-multi-platform-integration
date: 2024-05-05
category: Deployment
tags: [telegram, discord, multi-platform]
---

# 多平台整合

AI Agent 的價值在於其無處不在的接入能力。

## 1. Telegram Bot 整合
- **優點**：API 極其穩定，支援豐富的 UI 元件（Inline Buttons, MarkdownV2）。
- **實現**：使用 `python-telegram-bot` 框架，建立 Webhook 或 Long Polling。

## 2. Discord Bot 整合
- **優點**：適合社群互動，支援 Slash Commands 與 Thread 溝通。
- **實現**：使用 `discord.py`。

## 3. 同時多平台部署架構
要讓同一個 Agent 同時在 Telegram 與 Discord 運作，建議採用 **Gateway 模式**：
1. **Core Engine**：處理邏輯與工具呼叫。
2. **Platform Adapters**：各自負責處理不同平台的 Protocol（訊息格式轉換）。
3. **Shared State**：確保用戶在不同平台的互動能共享記憶。
