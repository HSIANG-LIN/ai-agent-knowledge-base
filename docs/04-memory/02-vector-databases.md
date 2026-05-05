---
title: 向量資料庫 (Vector Databases)
description: Vector DB 比較、Embedding 模型選擇、索引策略與設定實戰
tags: [memory, vector-db, embedding, retrieval]
---

# 向量資料庫 (Vector Databases)

## 為什麼需要 Vector DB？

傳統全文檢索 (FTS5) 只能比對關鍵字，無法理解語意。例如搜尋「Python 效能優化」不會匹配到「提升 CPython 執行速度」。Vector DB 透過 Embedding 將文字轉為語意向量，用 cosine similarity 找到概念相近的內容。

## 主流 Vector DB 比較

| 方案 | 部署 | 維度上限 | 索引 | 優點 | 缺點 |
|------|------|---------|------|------|------|
| **Chroma** | 嵌入式 (Embedded) | 無限制 | HNSW | 零配置、Python 原生 | 不適合生產規模 |
| **Qdrant** | 自架/雲端 | 65536 | HNSW | Rust 實作、效能好 | 需額外服務 |
| **Weaviate** | 自架/雲端 | 無限制 | HNSW+IVF | 內建模態、GraphQL API | 資源消耗高 |
| **Pinecone** | 雲端 SaaS | 無限制 | 自動 | 免運維 | 成本高、vendor lock |
| **pgvector** | PostgreSQL 插件 | 2000 | IVFFlat/HNSW | 整合既有 PG | 維度上限低 |
| **LanceDB** | 嵌入式 | 無限制 | IVF | 零配置、columnar storage | 較新、生態未成熟 |

## Embedding 模型選擇

| 模型 | 維度 | 語言 | 價格 |
|------|------|------|------|
| OpenAI text-embedding-3-small | 512/1536 | 多語言 | $0.02/1M tokens |
| OpenAI text-embedding-3-large | 256/1024/3072 | 多語言 | $0.13/1M tokens |
| Cohere embed-multilingual-v3.0 | 1024 | 多語言 | $0.10/1M tokens |
| BGE-M3 (Ollama) | 1024 | 多語言 | 免費 (本地) |
| intfloat/multilingual-e5-large | 1024 | 多語言 | 免費 (本地) |

## 索引策略

### IVFFlat (Inverted File with Flat)
- 先對向量空間分區 (K-means clustering)
- 搜尋時只掃描最近的 N 個分區
- 適合：百萬級別以下，追求 recall > 0.95

### HNSW (Hierarchical Navigable Small World)
- 多層圖結構，導航式搜尋
- 搜尋速度比 IVF 快 10-100x
- 適合：需要低延遲的即時場景
- 缺點：索引建置慢、記憶體消耗高

## pgvector 設定範例

```sql
CREATE EXTENSION vector;

CREATE TABLE memories (
  id SERIAL PRIMARY KEY,
  content TEXT,
  embedding vector(1536),
  metadata JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX ON memories USING hnsw (embedding vector_cosine_ops);
```

查詢：
```sql
SELECT content, 1 - (embedding <=> '[0.1, 0.2, ...]') AS similarity
FROM memories
ORDER BY embedding <=> '[0.1, 0.2, ...]'
LIMIT 5;
```
