# Hermes Agent 進階教學：串接 Telegram Bot

本教學將教你如何將 Hermes Agent 變成一個可以透過 Telegram 溝通的機器人。

## 步驟 1: 創建 Telegram Bot
1. 在 Telegram 中搜尋並找到 `@BotFather`。
2. 發送 `/newbot` 指令，並按照步驟設定 Bot 的名稱與 Username。
3. 完成後，你會取得一串 `HTTP API Token`，請妥善保存。

## 步驟 2: 配置環境變數
在你的專案根目錄（或 Hermes 設定目錄）的 `.env` 檔案中，新增以下設定：
```env
TELEGRAM_BOT_TOKEN=你的_TOKEN_放在這裡
TELEGRAM_HOME_CHANNEL=你的_CHAT_ID_或_CHANNEL_ID
```

## 步驟 3: 設定 Home Channel
`TELEGRAM_HOME_CHANNEL` 是 Agent 預設回覆訊息的目標。
*注意：如果是群組，請確保 Bot 已加入該群組。*

## 步驟 4: 啟動 Hermes Daemon
使用 `serve` 指令讓 Hermes 在背景執行，監聽 Telegram 訊息。
```bash
hermes serve
```
**預期輸出：**
看到類似 `Starting Telegram bot daemon...` 的啟動訊息。

## 步驟 5: 開始對話
打開 Telegram，對你的 Bot 發送訊息（例如：「你好」）。

## ✅ 驗證點
如果 Bot 在 Telegram 上自動回覆了你的訊息，表示串接成功。

## ⚠️ 常見坑點
- **Channel ID 錯誤：** 請注意 Telegram 的 `Channel ID` 或 `Group ID` 通常是以 `-` 開頭的數字（例如 `-100123456789`），不要誤用了 `Guild ID`（這是 Discord 的概念）。
- **require_mention 設定：** 如果你在群組中使用，預設可能需要 `@mention` Bot 才會觸發。若要讓它在群組中直接回應，請檢查相關設定。
