---
title: 記憶分類學 (Memory Taxonomy)
description: AI Agent 記憶的三層分類架構與 Hermes 對應實作
tags: [memory, taxonomy, architecture]
---

# 記憶分類學 (Memory Taxonomy)

## 三大記憶維度

### 程序性記憶 (Procedural Memory)
- **定義：** 「如何做」的記憶 — 技能、工具使用流程、SOP
- **Hermes 對應：** Skills (SKILL.md)、Tool Schemas、System Prompts
- **儲存：** 檔案系統 (`~/.hermes/skills/`)，YAML frontmatter + Markdown body
- **檢索：** 觸發詞匹配 (triggers)、語意搜尋
- **生命週期：** 手動建立 → GEPA 自動演化 → 版本疊代

### 情節性記憶 (Episodic Memory)
- **定義：** 「發生過什麼」的記憶 — 對話歷史、特定事件
- **Hermes 對應：** `session_search` (FTS5 全文檢索)、Context Window
- **儲存：** SQLite (`~/.hermes/sessions/`)，FTS5 索引
- **檢索：** 關鍵字搜尋、時間範圍過濾
- **生命週期：** 每次對話自動記錄 → session 壓縮 → 保留期限後清理

### 語意性記憶 (Semantic Memory)
- **定義：** 「是什麼」的記憶 — 事實、概念、常識
- **Hermes 對應：** MEMORY.md (2000 chars)、USER.md (1375 chars)、外部 Memory Provider
- **儲存：** 檔案系統 (bounded core) + Provider 後端 (SQLite/PostgreSQL/Vector DB)
- **檢索：** 自動注入 system prompt + Provider prefetch
- **生命週期：** 手動管理 (memory tool) + 自動萃取 (Provider 支援)

## 記憶生命週期

1. **編碼 (Encoding)：** Agent 從對話中萃取關鍵事實
2. **儲存 (Storage)：** 根據類型路由到對應儲存層
3. **檢索 (Retrieval)：** 必要時從多層同時召回相關記憶
4. **遺忘/壓縮 (Forgetting)：** Token 預算控制 + 摘要壓縮 + TTL 淘汰

## Hermes 記憶架構圖

```
┌─────────────────────────────────────┐
│         System Prompt               │
│  MEMORY.md + USER.md (always-on)    │
└──────────┬──────────────────────────┘
           │ auto-inject
┌──────────▼──────────────────────────┐
│      Memory Provider Layer          │
│  ┌──────┐ ┌──────┐ ┌─────────────┐ │
│  │FTS5  │ │Vector│ │Knowledge    │ │
│  │Search│ │Search│ │Graph        │ │
│  └──────┘ └──────┘ └─────────────┘ │
└─────────────────────────────────────┘
           │ prefetch + sync
┌──────────▼──────────────────────────┐
│       Tool Layer                    │
│  fact_store / session_search        │
└─────────────────────────────────────┘
```
