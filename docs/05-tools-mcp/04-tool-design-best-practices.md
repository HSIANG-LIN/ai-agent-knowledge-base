---
title: Tool 設計最佳實踐
description: 命名慣例、參數設計、錯誤處理、Rate Limiting 與 Retry 策略
tags: [mcp, tools, best-practices, design]
---

# Tool 設計最佳實踐

## 命名慣例

- **動詞開頭：** `search_files`、`send_message`、`analyze_data`
- **避免縮寫：** 不要用 `snd_msg`，用 `send_message`
- **一致性：** 同一領域的工具用一致的前綴 — `github_list_prs`、`github_create_issue`

## 參數設計準則

| 原則 | 說明 | 範例 |
|------|------|------|
| 必填最少化 | 只有絕對必要的才設 required | `keyword` 必填，`date_from` 可選 |
| 提供預設值 | 減少 LLM 需要決定的參數 | `source: "all"` |
| 使用 enum | 有限選項用 enum 而非 free text | `source: ["udn", "chinatimes", "all"]` |
| 參數說明 | 寫明格式與邊界條件 | `description: "日期格式 YYYY-MM-DD"` |

## 錯誤處理模式

### 使用者可理解的錯誤
```
✗ Bad Request
✓ 查無此股票代號「1234.TW」，請確認是上市（.TW）或上櫃（.TWO）格式
```

### 系統可恢復的錯誤
```python
try:
    result = await api_call()
except requests.Timeout:
    return "API 請求超時，請稍後重試。"
except requests.HTTPError as e:
    if e.response.status_code == 429:
        return "已達 API 速率限制，請等待 60 秒後再試。"
    return f"API 錯誤 ({e.response.status_code})：{e.response.text}"
```

## Rate Limiting 策略

```python
import time
from collections import deque

class RateLimiter:
    def __init__(self, max_calls: int, window: float):
        self.calls = deque()
        self.max_calls = max_calls
        self.window = window
    
    def acquire(self) -> bool:
        now = time.time()
        # 清除過期的呼叫記錄
        while self.calls and self.calls[0] < now - self.window:
            self.calls.popleft()
        
        if len(self.calls) >= self.max_calls:
            return False  # 超過限制
        
        self.calls.append(now)
        return True
```

## Retry 策略

1. **Transient errors：** 網路逾時、HTTP 503 → 重試 (最多 3 次)
2. **Non-transient errors：** 權限不足、參數錯誤 → 不回重試，直接回報
3. **Backoff：** 第一次等 1s、第二次 2s、第三次 4s (exponential backoff)
4. **Jitter：** 每次 delay 加入 ±20% 隨機抖動，避免 thundering herd
