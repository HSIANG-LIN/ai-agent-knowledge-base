# 快取與預算控制

## OpenRouter Response Caching
OpenRouter 提供了一定程度的緩存機制，透過配置 `cache_ttl` (Time To Live) 可以決定緩存回應的有效期，減少重複請求的成本與延遲。

## Prompt Caching
Prompt Caching 是降低長上下文成本的最有效手段：
- **Anthropic/vLLM 差異**：Anthropic 提供明確的 Prompt Caching API，對於重複使用的 Prefix 非常友好；vLLM 則透過自動化的 KV Cache 管理實現類似效果。
- **cache_ttl**：設定緩存的存續時間，對於固定系統指令（System Prompt）極其有效。

## Context Compression (上下文壓縮)
當對話歷史過長時，透過壓縮技術減少 Token 消耗：
- **Threshold / Target Ratio**：設定壓縮閾值或目標比例（例如將 Context 壓縮至原有的 50%）。
- **壓縮模型設定**：可以使用專門的小型模型進行摘要壓縮，而非直接截斷。

## Token 預算控制
為了防止 Agent 在死循環或複雜任務中消耗過多預算，應實施以下限制：
- **max_turns**：限制單次對話或單次任務的最大步驟數。
- **gateway_timeout**：設定 API 回應超時限制，避免長時間佔用資源。
- **tool_use_enforcement**：嚴格限制工具調用的頻率與類型，防止惡意或錯誤的循環調用。

## 實戰：如何將月費控制在 $10 以內
1. **優先使用低成本模型**：日常對話與簡單檢索全面切換至 `gpt-4o-mini` 或 `deepseek-v3`。
2. **強力執行 Prompt Caching**：將長達數千字的 System Prompt 與 Knowledge Base 內容置於緩存區。
3. **設置硬性 Token 預算**：在 Gateway 層級設置每日與每用戶的 Token 使用上限。
4. **路由策略**：僅在必要時（如遇到無法解決的複雜問題）才請求高階模型。
5. **定期清理記憶**：透過 Summarization 減少無效的歷史上下文。