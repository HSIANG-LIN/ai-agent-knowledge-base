---
title: Holographic Memory Provider
description: Hermes Holographic Memory — 零依賴、本機向量記憶的完整實作指南
tags: [memory, holographic, hrr, fact-store]
---

# Holographic Memory Provider

## 什麼是 Holographic Memory？

Holographic Memory 是 Hermes Agent 內建的最輕量 external memory provider。它不需要資料庫伺服器、不需要 API key，唯一依賴就是 SQLite（Python 內建）。

## HRR (Holographic Reduced Representations) 原理

HRR 是一種用固定維度向量表示結構化知識的數學方法：

- **綁定 (Binding)：** `CIRCLE = RED × SHAPE` — 兩個向量相乘，產生新的合成向量
- **疊加 (Superposition)：** 多個知識向量加總，壓縮到同一空間
- **檢索：** 透過相關性解綁，還原原始概念

相比傳統 embedding：
- ✅ 優點：可解釋性强、不需要訓練 embedding model、支援組合推理
- ❌ 缺點：維度有限、大量事實會互相干擾 (crosstalk)

## 安裝與設定

```bash
hermes config set memory.provider holographic
# 或互動式：
hermes memory setup  # 選擇 holographic
```

配置參數：
```yaml
memory:
  provider: holographic
plugins:
  hermes-memory-store:
    db_path: ~/.hermes/memory_store.db   # SQLite 路徑
    auto_extract: false                    # session 結束自動萃取
    default_trust: 0.5                     # 新事實預設信任分數
    hrr_dim: 1024                          # HRR 向量維度
```

## fact_store 工具操作指南

工具名稱為 `fact_store`，支援 9 種動作：

### 新增事實
```json
{
  "action": "add",
  "content": "Smith 住在台北市信義區象山站附近",
  "category": "user_pref",
  "tags": "location, Taipei"
}
```

### 語意搜尋
```json
{
  "action": "search",
  "query": "Smith 住在哪裡"
}
// 回傳：相關事實列表（依語意相似度排序）
```

### 實體探查
```json
{
  "action": "probe",
  "entity": "Smith"
}
// 回傳：所有與 Smith 相關的事實
```

### 組合推理
```json
{
  "action": "reason",
  "entities": ["Smith", "BenchMaster", "Stock Scanner"]
}
// 回傳：同時與這些實體相關的事實交集
```

### 矛盾偵測
```json
{
  "action": "contradict"
}
// 回傳：內容上相互矛盾的事實對
```

## Trust Scoring 機制

每個事實有 trust 值 (0.0 ~ 1.0)，影響檢索時排序：

- **新事實：** `default_trust` (預設 0.5)
- **feedback helpful：** trust +0.1 (上限 1.0)
- **feedback unhelpful：** trust -0.2 (下限 0.0)
- **低信任事實：** 仍然存在但排序靠後，不會完全遺失
