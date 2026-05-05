# Hermes Agent 進階教學：排程任務與自動化 (Cron)

透過 Cron 功能，你可以讓 Hermes Agent 在特定時間自動執行任務，例如每日早安報價或定期檢查系統狀態。

## 步驟 1: 建立排程任務
使用 `cronjob` 指令來建立一個任務。

**範例：每天早上 9 點發送早安訊息**
```bash
hermes cronjob action=create prompt="早安！現在是新的一天，請幫我整理今天的重點訊息。" schedule="0 9 * * *"
```
*指令說明：`0 9 * * *` 是標準的 Cron 表達式，代表每天 09:00。*

## 步驟 2: 確認排程列表
檢查目前系統中所有的排程任務，確保你的任務已成功建立。
```bash
hermes cronjob action=list
```
**預期輸出：**
你會看到一個任務列表，其中包含你剛剛建立的任務 ID。

## 步驟 3: 手動測試任務
如果你不想等到早上 9 點，可以用 `run` 指令手動觸發該任務來測試。
```bash
hermes cronjob action=run job_id=<你的_JOB_ID>
```

## ✅ 驗證點
確認在排程時間到來時，或者手動執行時，你會在預設的溝通頻道（如 Telegram）收到 Agent 產生的訊息。

## ⚠️ 常見坑點
- **Deliver 目標問題：** `deliver` 預設會將訊息送回「原本產生該排程的對話」。在 Cron 場景下，請確保設定了正確的 `TELEGRAM_HOME_CHANNEL`。
- **Prompt 必須自包含：** Cron 任務是在背景執行的，沒有當前的對話上下文。因此，你的 `prompt` 必須包含所有必要的資訊，不能依賴「像剛才那樣做」這種指令。
