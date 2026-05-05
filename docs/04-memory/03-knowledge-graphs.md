---
title: 知識圖譜 (Knowledge Graphs)
description: 知識圖譜在 AI Agent 中的應用，與 Vector DB 的比較及互補策略
tags: [memory, knowledge-graph, hindsight, entity-resolution]
---

# 知識圖譜 (Knowledge Graphs)

## 向量資料庫 vs 知識圖譜

| 面向 | Vector DB | Knowledge Graph |
|------|-----------|-----------------|
| **查詢方式** | 語意相似度 | 結構化關係查詢 |
| **最擅長** | 「找相似的」 | 「找關聯的」 |
| **推理能力** | 弱 (僅向量距離) | 強 (關係鏈推理) |
| **儲存結構** | 向量空間 | 節點 + 邊 + 屬性 |
| **更新成本** | 低 (直接 upsert) | 中 (需維護關係一致性) |
| **冷啟動** | 需要足夠向量才有效 | 少量節點即可推理 |

## 何時該用哪個？

- **Vector DB：** RAG、語意搜尋、推薦（「找出與這篇論文相似的文章」）
- **Knowledge Graph：** 關聯推理、問答鏈、事實驗證（「A 的作者還寫過哪些論文？這些論文分別被誰引用？」）
- **混合：** 先用 Vector DB 找到相關實體，再用 KG 探索關聯

## 實體識別與關係抽取

知識圖譜從非結構化文字中建構的關鍵步驟：

1. **NER (命名實體識別)：** 識別人物、組織、地點、時間等
2. **關係抽取：** 判斷實體間的關係 (published_by, works_at, located_in)
3. **消歧 (Disambiguation)：** 同名實體區分 (e.g., 蘋果公司 vs 蘋果水果)

## Hindsight Memory 的 KG 實作

Hindsight 是 Hermes 唯一內建知識圖譜的 memory provider：

- **自動萃取：** 每次 session 結束後，自動從對話中抽出實體與關係
- **Entity Resolution：** 將同義實體合併 (e.g., 「OpenAI」=「OpenAI Inc.」)
- **多策略檢索：** 同時使用關鍵字、向量、KG 路徑三種召回
- **`hindsight_reflect`：** 跨實體綜合推理 — 找出「A 與 B 之間透過哪些節點相關聯」

## 輕量替代方案

如果不想跑完整的 Hindsight daemon，可以用：

- **NetworkX (Python)：** 記憶體內圖運算，適合 prototyping
- **SQLite + Adjacency List：** 自幹輕量 KG，存 (subject, predicate, object) triples
- **Neo4j AuraDB Free：** 雲端 Neo4j，免費 50K 節點
