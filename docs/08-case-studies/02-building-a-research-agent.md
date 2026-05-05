---
title: 實戰：研究助理 Agent
description: 建立具備論文搜尋、摘要、知識庫建構能力的研究型 Agent
tags: [case-study, research, paper, knowledge-base]
date: 2026-05-05
---

# 實戰：研究助理 Agent

## 需求背景

需要一個能自動追蹤特定領域最新論文、產生摘要、並建立個人知識庫的 Agent。

## 實作流程

1. **論文監控：** 設定 cron job 每日搜尋 arXiv + blogwatcher RSS
2. **摘要 pipeline：** 新論文 → LLM 摘要 → 存入記憶庫
3. **問答介面：** 針對已存入的論文庫進行 RAG 查詢

## 使用技能

- `arxiv` — 論文搜尋與 metadata 擷取
- `youtube-content` — 相關演講/教學影片逐字稿分析
- `blogwatcher` — RSS feed 監控

## 擴展方向

- 加入知識圖譜（Hindsight）建立論文引用網絡
- 自動產生每週研究簡報
