# 模型路由策略

## 為什麼需要路由
在 Agent 系統中，模型路由（Model Routing）是優化成本與性能的核心手段。
- **簡單任務**：如摘要、格式轉換、基礎分類，使用便宜的模型（例如 GPT-4o-mini 或 DeepSeek-V3）即可滿足需求。
- **複雜推理**：如邏輯推導、代碼撰寫、多步規劃，則需要切換到強大的模型（例如 GPT-4o 或 Claude 3.5 Sonnet）。

## Hermes `smart_model_routing` 配置
Hermes 框架允許透過配置實現自動化的智能路由，根據任務特性動態分配模型。

## 分類策略
常見的路由維度包括：
1. **依輸入長度**：
   - `< 160 chars`: 判定為簡單指令，分配給 `cheap model`。
   - `> 160 chars`: 判定為需要處理內容，分配給 `mid-tier model`。
2. **依任務類型**：透過 Classifier 模型判斷任務屬於 `Reasoning`、`Coding` 還是 `Chat`。
3. **依用戶權限**：VIP 用戶使用高階模型，普通用戶使用基礎模型。

## Fallback Model 配置
為了確保系統的高可用性，必須配置 Fallback 模型。當 Primary 模型發生 API 錯誤、速率限制 (Rate Limit) 或超時時，系統會自動切換到指定的備用模型。

## 範例 Config
以下是一個典型的路由與回退配置範例：

```yaml
routing:
  strategy: smart_model_routing
  rules:
    - condition: "input_length < 160"
      primary: "deepseek-v3"
      fallback: "gpt-4o-mini"
    - condition: "task_type == 'complex_reasoning'"
      primary: "claude-3-5-sonnet"
      fallback: "gpt-4o"
  default:
    primary: "deepseek-v3"
    fallback: "gpt-4o-mini"
```