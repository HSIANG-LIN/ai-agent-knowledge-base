---
title: 工具呼叫生命週期
date: 2026-05-05
tags: [patterns, tool-calling]
---

# 工具呼叫生命週期 (Tool Calling Lifecycle)

工具呼叫 (Tool Calling) 是 AI Agent 從「說話者」轉變為「執行者」的核心機制。一個完整的工具呼叫循環包含以下階段：

## 1. 工具定義 (Definition)
在與 LLM 溝通前，必須明確定義工具的「契約」。
*   **名稱 (Name):** 具備語意化的名稱（例如 `get_weather` 而非 `func1`）。
*   **描述 (Description):** 告訴模型「什麼時候」該用這個工具，「為什麼」要用它。
*   **參數 Schema (JSON Schema):** 定義參數的類型 (type)、是否必填 (required) 以及預期格式。

## 2. 決策階段 (Reasoning & Decision)
模型接收到 User Prompt 後，進行推理：
*   **意圖識別:** 使用者是否要求執行外部操作？
*   **工具匹配:** 在可用工具集中，哪一個工具最能滿足需求？
*   **參數提取:** 從對話上下文中提取並格式化工具所需的參數。

## 3. 生成呼叫 (Generation)
模型不直接執行代碼，而是輸出一個結構化的指令（通常是 JSON 格式），包含：
*   `call_id`: 用於追蹤與回饋。
*   `function_name`: 要執行的工具名。
*   `arguments`: 提取出的參數。

## 4. 解析與執行 (Parsing & Execution)
這是系統（而非模型本身）的工作：
*   **解析:** 系統解析 LLM 輸出的 JSON。
*   **驗證:** 檢查參數是否符合 Schema。
*   **執行:** 呼叫本地代碼、API 或外部服務。

## 5. 回饋與總結 (Feedback & Summarization)
*   **結果回傳:** 將工具執行的結果（成功訊息或錯誤訊息）以特定的格式餵回給模型。
*   **最終整合:** 模型根據工具回傳的資訊，重新整理語言，回答使用者的問題或進行下一步動作。

## 6. 進階機制

### 並行呼叫 (Parallel Tool Calling)
現代模型（如 GPT-4, Claude 3.5）支持在單次推理中決定同時呼叫多個工具。例如：「幫我查東京和倫敦的天氣」，模型會一次性輸出兩個工具呼叫指令，大幅提升效率。

### 錯誤處理 (Error Handling)
當工具執行失敗時，系統不應直接崩潰，而應：
1.  **捕捉錯誤:** 捕捉 Exception。
2.  **結構化回傳:** 將錯誤訊息（例如 `Error: invalid date format`）作為工具回傳結果。
3.  **自我修正:** 模型看到錯誤後，會嘗試修正參數並重新發起呼叫。

## 流程總結圖

`User Prompt` $\rightarrow$ `LLM (Reasoning)` $\rightarrow$ `Tool Call (JSON)` $\rightarrow$ `System (Execution)` $\rightarrow$ `Tool Result` $\rightarrow$ `LLM (Summarization)` $\rightarrow$ `Final Response`
