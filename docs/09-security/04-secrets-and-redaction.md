---
title: 機密管理與遮蔽 (Secrets and Redaction)
date: 2026-05-06
category: Security
tags: [secrets, redaction, env, security]
---

# 機密管理：如何保護您的 API Keys 與密碼

在開發 AI Agent 時，最常見的安全失誤之一就是不小心將敏感資訊（如 OpenAI API Key、資料庫密碼、AWS Access Key）洩露到日誌 (Logs)、版本控制系統 (Git) 或對話紀錄中。

## 1. .env 檔案最佳實踐

`.env` 檔案用於在本地開發環境存儲敏感變數。

**✅ 應該做的事：**
- **建立 `.gitignore`**：務必將 `.env` 加入 `.gitignore`，絕對不要 commit 它。
- **使用 `.env.example`**：建立一個範例檔案，只列出 Key 的名稱而不包含真實數值，方便團隊成員參考。
- **最小權限原則**：只存放該 Agent 執行任務所必須的 Key。

**❌ 絕對不要做的事：**
- **將密碼寫在程式碼中**：例如 `api_key = "sk-..."`。
- **Commit 到 Git**：一旦 Commit，即便之後刪除，歷史紀錄中依然可以輕易找回。

## 2. 環境變數注入 `config.yaml`

Hermes 支援透過環境變數來配置 `config.yaml`，這可以避免在設定檔中直接寫死敏感資訊。

**實作方法：**
在 `config.yaml` 中使用 `${VARIABLE_NAME}` 語法。

```yaml
# config.yaml 範例
llm:
  provider: "openai"
  api_key: "${OPENAI_API_KEY}"  # Hermes 會自動從系統環境變數讀取

database:
  password: "${DB_PASSWORD}"
```

在執行時，確保環境中已有這些變數：
```bash
export OPENAI_API_KEY="sk-xxxx..."
hermes-agent start
```

## 3. Hermes `redact_secrets` 機制

為了防止敏感資訊在 Agent 的思考過程 (Thought) 或工具回傳結果 (Tool Output) 中洩露到日誌系統，Hermes 提供內建的遮蔽機制。

**配置方式：**
```yaml
security:
  redact_secrets: true
  # 可自定義偵測模式 (Regex)
  redaction_patterns:
    - "sk-[a-zA-Z0-9]{32,}"  # 針對 OpenAI Key
    - "AIza[0-9A-Za-z-_]{35}" # 針對 Google API Key
```

**運作原理：**
當 Agent 執行 `print(api_key)` 時，Hermes 的日誌攔截器會比對 pattern，並將輸出替換為 `[REDACTED]`。

## 4. git-credentials 安全儲存

當 Agent 需要執行 Git 操作（如 `git push`）時，必須處理身份驗證。

- **推薦方式**：使用 SSH Key 並設定 `ssh-agent`，而非使用 HTTPS 帳號密碼。
- **Token 使用**：若必須使用 HTTPS，請使用 Personal Access Token (PAT)，並設定其有效期與權限範圍。

## 5. 踩坑提醒：WSL 下的 Credential Store

在 Windows Subsystem for Linux (WSL) 環境下使用 Git 時，經常會遇到 credential 存取錯誤。

**常見問題：**
當您在 WSL 中配置 `git config --global credential.helper store` 時，Git 會將憑證以純文字形式存放在 `~/.git-credentials`。

**關鍵注意事項：**
如果您嘗試與 Windows 端的 Git Credential Manager 整合，請記住：
> **WSL 內的 credential store 格式必須明確包含 username**。

如果格式錯誤，Agent 在嘗試自動化 Git 操作時會卡在「要求輸入密碼」的提示畫面，導致程序掛起。建議在 WSL 內獨立管理專屬的 SSH Key 以避免混淆。
