---
title: 建立工具 (Building Tools)
description: Tool 設計原則、Function Calling Schema 與 FastMCP Server 實作範例
tags: [mcp, tools, development, fastmcp]
---

# 建立工具 (Building Tools)

## Tool 設計原則

1. **單一職責：** 一個工具只做一件事，並做到最好
2. **明確 Schema：** 參數用語意化的名稱，description 寫清楚格式與邊界條件
3. **好的錯誤訊息：** 不要只回 `Error occurred`，要包含原因與修復建議
4. **冪等性：** 同一參數重複呼叫應產生相同結果（或無副作用）
5. **輸入驗證：** 在 tool 內部驗證參數，不要假設 LLM 永遠產生正確格式

## Function Calling Schema 格式

```json
{
  "name": "search_news",
  "description": "搜尋新聞文章。支援日期範圍、關鍵字、來源過濾。",
  "parameters": {
    "type": "object",
    "properties": {
      "keyword": {
        "type": "string",
        "description": "搜尋關鍵字（必填）"
      },
      "date_from": {
        "type": "string",
        "description": "起始日期，格式 YYYY-MM-DD"
      },
      "date_to": {
        "type": "string",
        "description": "結束日期，格式 YYYY-MM-DD"
      },
      "source": {
        "type": "string",
        "enum": ["udn", "chinatimes", "all"],
        "description": "新聞來源"
      }
    },
    "required": ["keyword"]
  }
}
```

## FastMCP Server 範例

```python
from fastmcp import FastMCP

mcp = FastMCP("news-server")

@mcp.tool()
def search_news(keyword: str, date_from: str = None, date_to: str = None) -> str:
    """搜尋新聞文章。支援日期範圍、關鍵字過濾。"""
    import requests
    
    params = {"keyword": keyword}
    if date_from:
        params["date_from"] = date_from
    if date_to:
        params["date_to"] = date_to
    
    resp = requests.get("https://api.news-service.com/search", params=params)
    resp.raise_for_status()
    
    results = resp.json()["articles"]
    if not results:
        return "查無符合條件的新聞。"
    
    lines = [f"- [{r['title']}]({r['url']}) ({r['date']})" for r in results[:10]]
    return "\n".join(lines)

if __name__ == "__main__":
    mcp.run()
```

## 測試策略

1. **Unit Test Tool Logic：** 直接測試工具函數（不經 MCP transport）
2. **Integration Test MCP Server：** 用 `mcporter` 或 MCP client SDK 測試完整 call chain
3. **Error Injection：** 測試網路斷線、參數無效、rate limit 等邊界情況
