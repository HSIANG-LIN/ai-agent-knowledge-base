# Hermes Agent 進階教學：長期記憶與 MCP 工具

本教學將帶你探索如何讓 Agent 擁有「記憶」以及透過 MCP (Model Context Protocol) 擴充功能。

## 步驟 1: 開啟記憶功能
將記憶提供商設定為 `holographic`，這能讓 Agent 具備更強大的長期記憶能力。
```bash
hermes config set memory.provider holographic
```

## 步驟 2: 確認記憶狀態
檢查目前的記憶設定是否已生效。
```bash
hermes memory status
```

## 步驟 3: 手動新增事實 (Fact Store)
你可以主動告訴 Agent 一些事實，讓它記住。
```bash
hermes fact_store add "我的生日是 5 月 6 日"
```

## 步驟 4: 安裝 MCP Server
MCP 允許 Agent 使用外部工具（如讀取你的檔案系統）。
安裝 `filesystem` MCP server：
```bash
# 範例指令，實際安裝方式依 MCP 提供商而定
hermes mcp install filesystem
```

## 步驟 5: 測試 MCP 工具與記憶
進行跨 Session 的測試：
1. **測試記憶：** 開啟一個新對話，問：「我的生日是什麼時候？」
2. **測試 MCP：** 要求 Agent：「列出我當前目錄下的所有檔案。」

## ✅ 驗證點
- **記憶驗證：** Agent 能正確回答你在步驟 3 設定的事實，且不需再次提醒。
- **MCP 驗證：** Agent 能成功列出檔案，而不是說「我無法讀取你的檔案」。

## ⚠️ 常見坑點
- **Holographic 與 Hindsight：** `holographic` 提供極佳的零配置體驗，但如果你需要更深層的檢索（Hindsight），通常需要額外配置 LLM API Key 來處理向量檢索。
- **MCP 權限：** 如果 MCP 工具無法執行，請檢查該 MCP Server 是否有讀取目標路徑的作業系統權限。
