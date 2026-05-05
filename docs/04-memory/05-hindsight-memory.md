---
title: Hindsight Memory Provider
description: Hermes Hindsight — 知識圖譜驅動的長期記憶，支援三種部署模式
tags: [memory, hindsight, knowledge-graph, local-embedded]
---

# Hindsight Memory Provider

## 概覽

Hindsight 是功能最完整的 Hermes memory provider，特色是內建知識圖譜、多策略檢索、跨記憶推理。

## 三種部署模式

### 1. Cloud Mode
- **連線到：** Hindsight Cloud API (ui.hindsight.vectorize.io)
- **需要：** API key
- **優點：** 零維運、即開即用
- **適合：** 不想管 infra 的場景

### 2. Local Embedded (推薦)
- **架構：** Hermes 自動啟動本地 Hindsight daemon + 內建 PostgreSQL
- **需要：** LLM API key（支援 Ollama/OpenAI/Claude/LM Studio）
- **優點：** 資料完全本地、daemon 5 分鐘閒置自動停止、web UI 可視化
- **適合：** 注重隱私、自架環境

### 3. Local External
- **連線到：** 自建的 Hindsight 實例 (Docker 或 bare metal)
- **需要：** 預先部署好的 Hindsight server
- **適合：** 已有 Hindsight 基礎建設的團隊

## 配置範例 (Local Embedded)

```yaml
memory:
  provider: hindsight

# ~/.hermes/hindsight/config.json
{
  "mode": "local_embedded",
  "llm_provider": "openai",
  "llm_model": "gpt-4o-mini",
  "auto_recall": true,
  "auto_retain": true,
  "recall_budget": "mid",
  "recall_prefetch_method": "reflect",
  "retain_every_n_turns": 3
}
```

## 核心機制

### Auto Recall (自動預取)
每次 agent 收到訊息前，background 自動從 Hindsight 召回相關記憶注入 context。

### Auto Retain (自動儲存)
每次 assistant 回應後，將對話內容非同步傳送給 Hindsight 進行事實萃取。

### 多策略檢索
- **BM25 全文檢索：** 傳統關鍵字匹配
- **向量相似度：** 語意層級搜尋
- **知識圖譜路徑：** 實體關係探索

### `hindsight_reflect` 工具
跨記憶綜合推理。給定一個 query，Hindsight 會：
1. 召回多條相關記憶
2. 用 LLM 綜合分析找出模式或矛盾
3. 回傳結構化反思結果

## 啟動 Web UI

```bash
hindsight-embed -p hermes ui start
```

可視化管理記憶庫、檢視知識圖譜、手動調整事實。
