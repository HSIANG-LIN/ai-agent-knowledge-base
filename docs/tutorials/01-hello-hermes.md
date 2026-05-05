# Hermes Agent 入門教學：從零開始

本教學將引導你完成 Hermes Agent 的首次安裝與啟動。

## 步驟 1: 安裝 Hermes Agent
使用 `pip` 安裝核心套件。
```bash
pip install hermes-agent
```
**預期輸出：**
看到 `Successfully installed hermes-agent-...` 的訊息。

## 步驟 2: 初始設定
執行設定指令來配置你的 LLM 提供商。
```bash
hermes setup
```
**操作指南：**
1. 當系統詢問 Provider 時，請選擇 `OpenRouter`。
2. 依照提示輸入你的 `OpenRouter API Key`。

## 步驟 3: 進行第一次對話
測試 Agent 是否能正常運作。
```bash
hermes chat "Hello，你是誰？"
```
**預期輸出：**
Agent 會回覆類似「你好！我是 Hermes Agent...」的訊息。

## ✅ 驗證點
只要你在終端機看到 Agent 的文字回應，即代表安裝與 API 配置成功。

## ⚠️ 常見坑點
- **PATH 沒設：** 如果輸入 `hermes` 顯示 `command not found`，請檢查 Python 的 `bin` 目錄是否已加入到你的系統環境變數 `PATH` 中。
- **API Key 無效：** 如果回應出現錯誤訊息（如 401 Unauthorized），請檢查 API Key 是否複製完整，或 OpenRouter 帳戶是否有餘額。
