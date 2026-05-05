---
title: 常用 MCP Servers
description: 常用 MCP Servers 介紹、配置範例與安全考量
tags: [mcp, servers, configuration, security]
---

# 常用 MCP Servers

## 官方/社群常用 Servers

| Server | 用途 | 安裝方式 |
|--------|------|---------|
| **filesystem** | 檔案讀寫、目錄操作 | `npx @modelcontextprotocol/server-filesystem` |
| **github** | PR、Issue、Code Search | `npx @modelcontextprotocol/server-github` |
| **sqlite** | 資料庫查詢 | `npx @modelcontextprotocol/server-sqlite` |
| **playwright** | 瀏覽器自動化 | `npx @modelcontextprotocol/server-playwright` |
| **brave-search** | 網頁搜尋 | `npx @modelcontextprotocol/server-brave-search` |
| **puppeteer** | 網頁渲染 + 截圖 | `npx @modelcontextprotocol/server-puppeteer` |

## Hermes config.yaml 配置範例

```yaml
mcp:
  servers:
    # 本機檔案系統工具
    - name: filesystem
      command: npx
      args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    
    # GitHub 工具（需要 token）
    - name: github
      command: npx
      args: ["-y", "@modelcontextprotocol/server-github"]
      env:
        GITHUB_PERSONAL_ACCESS_TOKEN: "${GITHUB_TOKEN}"
    
    # 搜尋引擎
    - name: brave-search
      command: npx
      args: ["-y", "@modelcontextprotocol/server-brave-search"]
      env:
        BRAVE_API_KEY: "${BRAVE_API_KEY}"
```

## 安全考量

1. **最小權限原則：** filesystem server 只掛載需要的目錄，不要掛 `/`
2. **環境變數注入：** API keys 透過 env 傳遞，不要寫死在 config
3. **Sandbox 執行：** 敏感操作使用隔離容器或子使用者
4. **Input Sanitization：** 不要信任 LLM 產生的參數 — 驗證路徑、防 path traversal
5. **Rate Limiting：** 遠端 API 類工具一定要有 rate limit 保護
