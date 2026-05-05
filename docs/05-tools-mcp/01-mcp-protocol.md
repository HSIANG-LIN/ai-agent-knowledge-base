---
title: MCP 協定 (Model Context Protocol)
description: MCP 的架構、Transport、核心操作與 Hermes 實作
tags: [mcp, protocol, tools, integration]
---

# MCP 協定 (Model Context Protocol)

## MCP 是什麼？

MCP 是由 Anthropic 提出的開放標準，為 AI Agent 提供統一的工具和資源存取介面。類似 USB-C 為硬體提供的標準化連接 — 任何 MCP-compatible 的 Agent 都可以使用任何 MCP Server 提供的工具。

## 架構

```
┌──────────┐     MCP Protocol      ┌──────────┐
│  Host    │ ◄──────────────────►  │  Server  │
│ (Agent)  │     stdio / HTTP/SSE  │  (Tools) │
└──────────┘                       └──────────┘
```

- **Host：** AI Agent (e.g., Hermes、Claude Desktop)
- **Client：** Host 內建的 MCP client，負責連線管理
- **Server：** 提供工具、資源、prompts 的輕量服務

## Transport 層

### stdio (標準輸入輸出)
- Server 作為子程序啟動
- 透過 stdin/stdout 傳遞 JSON-RPC 訊息
- 適合本機工具 (filesystem、SQLite)

### HTTP/SSE (Server-Sent Events)
- Server 作為 HTTP 服務獨立運行
- 支援遠端存取、多 client 共享
- 適合雲端工具、團隊共享

## 核心操作 (JSON-RPC)

### ListTools
```json
{
  "jsonrpc": "2.0",
  "method": "tools/list",
  "params": {},
  "id": 1
}
```
回傳該 server 提供的所有工具清單。

### CallTool
```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "search_files",
    "arguments": { "pattern": "*.py" }
  },
  "id": 2
}
```
執行指定工具並回傳結果。

## Hermes Agent 的 MCP 實作

Hermes 內建兩層 MCP 支援：

### 1. Native MCP Client
在 `config.yaml` 中設定 MCP servers，工具自動註冊到 Agent 的 toolset：
```yaml
mcp:
  servers:
    - name: filesystem
      command: npx
      args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user"]
```

### 2. mcporter CLI Bridge
外部指令列工具，用於 ad-hoc MCP 操作：
```bash
mcporter list github    # 列出 github server 的所有工具
mcporter call github search_code "query: lang:python requests"
```
