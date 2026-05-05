---
title: 工具使用 (Tool Use)
description: 理解 Agent 如何透過 Function Calling 與外部工具進行實質行動。
tags: [AI-Agent, Tool Use, Function Calling, MCP]
---

# 工具使用 (Tool Use)

如果說 LLM 是 Agent 的大腦，那麼「工具 (Tools)」就是 Agent 的手腳。透過工具，Agent 能夠突破語言模型的限制，實現存取即時資訊、操作檔案系統、執行程式碼或控制外部硬體的能力。

## Tool 的定義：Function Calling Schema

在現代 Agent 開發中，工具通常透過 **Function Calling Schema** 來定義。這是一份結構化的說明文件，告訴 LLM 該工具能做什麼、需要什麼參數。

一個完整的 Tool Schema 通常包含：

*   **Name (名稱)**：工具的唯一識別碼（例如：`search_web`）。
*   **Description (描述)**：極其重要的部分。LLM 依賴此描述來判斷何時該使用此工具。描述必須精確且包含使用場景。
*   **Parameters (參數)**：定義輸入參數的類型 (Type)、必填性 (Required) 以及詳細描述 (JSON Schema 格式)。

## Tool Discovery：如何發現工具

Agent 知道有哪些工具可用，通常有兩種方式：

1.  **靜態註冊 (Static Registration)**：在啟動 Agent 時，開發者直接將工具列表硬編碼或透過設定檔注入到 System Prompt 中。
2.  **動態 MCP Discovery**：利用 **Model Context Protocol (MCP)** 等標準協定，讓 Agent 能在執行期間動態地從伺服器端發現、連結並使用新的工具，極大地提升了擴展性。

## Tool Execution Pipeline (執行流水線)

當 Agent 決定使用工具時，會經歷以下標準流程：

1.  **意圖匹配 (Intent Matching)**：LLM 根據用戶需求，從工具清單中挑選出最合適的工具並生成參數。
2.  **Schema 驗證 (Schema Validation)**：系統檢查 LLM 生成的參數是否符合預定義的 JSON Schema（例如：類型是否正確、必填欄位是否遺漏）。
3.  **執行 (Execution)**：系統調用實際的程式碼或 API 來執行任務。
4.  **結果回饋 (Observation Feedback)**：將工具執行後的結果（成功訊息或錯誤訊息）包裝成觀察值，送回給 LLM 進行下一輪推理。

## 安全性與防禦機制

給予 Agent 操作工具的能力意味著巨大的安全風險。必須建立防禦牆：

*   **Tirith 策略引擎**：在執行前進行動態權限檢查，判斷該行動是否符合安全策略。
*   **命令白名單 (Command Whitelist)**：限制 Agent 只能執行特定範圍內的命令（例如：只能讀取目錄，不能刪除檔案）。
*   **沙盒執行 (Sandbox Execution)**：將具有風險的操作（如執行 Python 代碼、Shell 指令）放在隔離的容器或沙盒環境中運行，防止對宿主系統造成破壞。

## Hermes Agent 的工具系統

Hermes Agent 提供了一套分層的工具架構：

*   **Built-in Tools**：內建於核心的基礎工具（如：環境變數讀取、基本檔案操作）。
*   **Plugin System**：允許開發者透過特定介面撰寫自定義插件。
*   **MCP Support**：支援標準的 Model Context Protocol，可無縫接入現有的 MCP 生態系工具。
