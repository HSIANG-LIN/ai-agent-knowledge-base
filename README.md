# 🤖 AI Agent Knowledge Base

> 一份專注於 **AI Agent 架構、框架、模式、記憶系統與部署** 的實戰知識庫。
> 從原理到實作，從選擇框架到部署上線，涵蓋完整生態。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)

---

## 📋 內容總覽

```
├── docs/
│   ├── getting-started/     # 快速入門：安裝指南、實用 Skills 推薦（含踩坑記錄）
│   ├── 01-fundamentals/     # 核心概念：什麼是 Agent、架構、規劃推理、工具使用、記憶系統
│   ├── 02-frameworks/       # 框架深度解析：Hermes Agent、LangChain、CrewAI、比較與選擇
│   ├── 03-patterns/         # 設計模式：ReAct、Plan-Execute、Reflection、Multi-Agent、Tool Calling、RAG
│   ├── 04-memory/           # 記憶系統：記憶分類學、Vector DB、知識圖譜、Holographic、Hindsight
│   ├── 05-tools-mcp/        # MCP 協定：協議解析、工具設計、Server 配置、最佳實踐
│   ├── 06-deployment/       # 部署指南：本機、VPS、Docker、多平台（Telegram/Discord）
│   ├── 07-advanced/         # 進階主題：自我演化 (GEPA)、向量記憶深度分析、多 Agent 協調
│   └── 08-case-studies/     # 實戰案例：Hermes 生態全景、股票掃描器
├── assets/diagrams/         # 架構圖（開瀏覽器即可檢視的 HTML）
└── references/              # 論文清單與外部資源
```

---

## 🚀 快速導覽

| 如果你是... | 建議從這裡開始 |
|------------|--------------|
| **剛要開始裝 Hermes** | [01-installation.md](docs/getting-started/01-installation.md) → [02-useful-skills.md](docs/getting-started/02-useful-skills.md) |
| **初次接觸 AI Agent** | [01-what-is-an-ai-agent.md](docs/01-fundamentals/01-what-is-an-ai-agent.md) → [02-agent-architecture.md](docs/01-fundamentals/02-agent-architecture.md) |
| **想選一個框架** | [05-selecting-framework.md](docs/02-frameworks/05-selecting-framework.md) → [04-comparison.md](docs/02-frameworks/04-comparison.md) |
| **想理解記憶怎麼設計** | [01-memory-taxonomy.md](docs/04-memory/01-memory-taxonomy.md) → [04-holographic-memory.md](docs/04-memory/04-holographic-memory.md) |
| **想部署上線** | [01-local-setup.md](docs/06-deployment/01-local-setup.md) → [02-vps-deployment.md](docs/06-deployment/02-vps-deployment.md) |
| **想實作多 Agent 系統** | [04-multi-agent.md](docs/03-patterns/04-multi-agent.md) → [03-multi-agent-orchestration.md](docs/07-advanced/03-multi-agent-orchestration.md) |
| **想讓 Agent 自我進化** | [01-self-evolution.md](docs/07-advanced/01-self-evolution.md) + [assets/diagrams/agent-architecture.html](assets/diagrams/agent-architecture.html) |

---

## 🏗 架構圖

知識庫包含 3 個互動式 HTML 架構圖（開瀏覽器即可檢視，無需任何依賴）：

| 圖表 | 說明 |
|------|------|
| [agent-architecture.html](assets/diagrams/agent-architecture.html) | Hermes Agent 完整架構 — Gateway → Session → Memory/Tools/Skills/Delegation → LLM |
| [memory-pyramid.html](assets/diagrams/memory-pyramid.html) | 記憶金字塔 — SQLite → Vector DB → Knowledge Graph，各層對應 Hermes Provider |
| [tool-use-flow.html](assets/diagrams/tool-use-flow.html) | Tool 呼叫流程 — User Input → Schema Validate → Execute → Feedback Loop |

---

## 📖 核心技術

### Hermes Agent
本知識庫以 **[Hermes Agent](https://github.com/NousResearch/hermes-agent)**（Nous Research）為主要實作參考，因為它是目前少數具備完整學習閉環（Skill 建立 → 記憶儲存 → 自我演化）的開源 Agent 框架。

### 記憶系統
涵蓋從最輕量的 **Holographic Memory**（SQLite + HRR，零依賴）到功能最完整的 **Hindsight**（知識圖譜 + 多策略檢索），以及 Vector DB、全文檢索的比較與選擇。

### 工具與 MCP
深入解析 Model Context Protocol 的架構與實作，包含如何設計、開發、部署 MCP Server。

---

## 🤝 貢獻

歡迎提交 PR！直接 fork 後發 PR，或開 Issue 討論想新增的方向。

內容方向：
- 修正錯誤或補充遺漏的實作細節
- 新增其他框架（AutoGPT、Semantic Kernel 等）的分析
- 新增實戰案例
- 改善架構圖

---

## 📚 參考資源

參見 [references/papers.md](references/papers.md) 取得完整的論文列表與外部連結。

---

## 📄 License

MIT License — 可自由使用、修改、分享。
