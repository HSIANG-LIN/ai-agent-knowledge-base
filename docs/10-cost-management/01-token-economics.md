# Token 計費模型完全解析

## 各 Provider 計價比較 (估算值)

| Provider | 模型範例 | Input (per 1M) | Output (per 1M) |
| :--- | :--- | :--- | :--- |
| **OpenRouter** | 聚合各家價格 | 視模型而定 | 視模型而定 |
| **OpenAI** | GPT-4o | ~$5.00 | ~$15.00 |
| **OpenAI** | GPT-4o-mini | ~$0.15 | ~$0.60 |
| **Anthropic** | Claude 3.5 Sonnet | ~$3.00 | ~$15.00 |
| **Google Gemini** | Gemini 1.5 Pro | ~$3.50 | ~$10.50 |

## Input vs Output Token 價格差異
通常 **Output token 的價格會比 Input token 貴 3 到 4 倍**。這是因為生成文本（Autoregressive generation）需要消耗更多的計算資源與顯存帶寬。

## Context Window 與成本
Context window 的大小直接影響成本，特別是處理長文本時：
- **Full-cache**: 將整個 context 緩存，下次使用時極大降低輸入成本。
- **Partial-cache**: 僅緩存部分內容，適合頻繁變動的對話。
- 256K context 的長文本如果不使用 caching 技術，單次請求的成本會非常驚人。

## 隱藏成本
- **Tool call tokens**: 當模型進行工具調用時，產生的 JSON 格式與思考過程會消耗大量 tokens。
- **System prompt 注入**: 複雜的 System prompt 會在每次對話中重複計費。
- **Memory prefetch**: 為了提供上下文，系統預先檢索並載入的記憶體內容也會計費。

## 實例計算：一次 ReAct 循環的成本估算
假設使用 GPT-4o，一次典型的 ReAct (Reasoning + Acting) 循環：
1. **Input**: 1000 tokens (System + History + Query)
2. **Reasoning**: 200 tokens (Thought)
3. **Tool Call**: 100 tokens (Function call)
4. **Tool Result**: 500 tokens (Observation)
5. **Final Answer**: 200 tokens (Response)

**總計**: Input 1600, Output 500. 
估算成本 = $(1.6 \times 0.005) + (0.5 \times 0.015) = \$0.008 + \$0.0075 = \$0.0155$。