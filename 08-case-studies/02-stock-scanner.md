---
title: 02-stock-scanner-case-study
date: 2024-05-05
category: Case Studies
tags: [case-study, stock-scanner, automation]
---

# 實戰案例：股票掃描器 (Stock Scanner)

這是一個結合了資料抓取、分析與自動發送訊息的完整 Agent 流程。

## 1. 系統架構
- **Data Ingestion**：使用 `yfinance` 進行批量下載；若遇到 API 限制，自動 fallback 到 `FinMind`。
- **Analysis Engine**：計算移動平均線 (MA)、RSI 等技術指標。
- **Delivery Layer**：透過 Discord Webhook 將掃描結果推送到指定的頻道。

## 2. 自動化工作流 (Workflow)
1. **Cron Scheduling**：利用 Linux Crontab 或 Python `schedule` 庫，設定每日收盤後啟動。
2. **Execution**：Agent 啟動 $\rightarrow$ 下載數據 $\rightarrow$ 篩選標的 $\rightarrow$ 生成摘要。
3. **Notification**：發送訊息 $\rightarrow$ 任務結束 $\rightarrow$ 進入睡眠。

## 3. 關鍵技術點
- **Error Handling**：處理網路斷線與 API 限制。
- **State Management**：記錄上次掃描的時間點，避免重複處理。
