---
title: Stock Scanner 實戰案例
description: 從需求到實現 — 自動化股票掃描器的完整開發歷程
tags: [case-study, stock, scanner, yfinance, discord]
date: 2026-05-05
---

# Stock Scanner 實戰案例

## 專案背景

建立一個每日自動掃描台股、找出潛在交易標的的系統，並透過 Discord 推送結果給使用者。

## 架構設計

```
┌──────────┐   ┌──────────────┐   ┌──────────┐   ┌───────────┐
│yfinance  │ → │ Strategy     │ → │ Notifier │ → │ Discord   │
│Data Prov │   │ Engine       │   │ (Filter) │   │ Channel   │
└──────────┘   └──────────────┘   └──────────┘   └───────────┘
     ↑                ↑                 ↑              ↑
 Batch Download   6 Strategies     Score/Filter    Cron Schedule
 .TW/.TWO                          Top N Results   Daily 15:00
```

## 關鍵決策

### 資料源：yfinance > FinMind
- 原本使用 FinMind API，但遭遇 402 rate limit
- 重構成 yfinance 為主要資料源（batch download，一次 100 檔，間隔 5 秒）
- `.TW`（上市）和 `.TWO`（上櫃）雙後綴支援
- FinMind 降級為最後補抓備援，且有 rate-limit 自動偵測

### 傳送：Discord only
- Telegram 作為開發溝通頻道，股票推送只走 Discord
- Systemd cron job 觸發，script 內建 Notifier 處理發送

## 策略引擎

6 個選股策略同時運作，各自產出分數後加權排序。

## 覆蓋率提升

從 FinMind 時期約 25% 覆蓋率，透過 yfinance batch 模式提升至 73%。
