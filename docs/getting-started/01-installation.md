---
title: Hermes Agent 安裝與配置
description: 從零到上手的完整安裝指南，含 WSL、Docker、常見踩坑記錄
tags: [installation, setup, configuration, wsl, docker]
date: 2026-05-05
---

# Hermes Agent 安裝與配置

## 安裝方式

### 方式一：pip 安裝（推薦，快速穩定）

```bash
pip install hermes-agent
hermes setup          # 互動式初始化
hermes chat           # 測試是否正常運作
```

### 方式二：原始碼安裝（適合開發者）

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
pip install -e .
hermes setup
```

### 方式三：uv 安裝（更快）

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
uv pip install -e .
hermes setup
```

## 初始化設定

執行 `hermes setup` 後，精靈會引導你完成：

1. **選擇 LLM Provider** — 建議選 OpenRouter（不需申請多個 API key）
2. **輸入 API Key** — 貼上你的 key
3. **選擇預設模型** — 如 `deepseek/deepseek-v4-flash`（便宜又快）
4. **選擇平台** — Telegram / Discord / CLI

設定檔位置：`~/.hermes/config.yaml`

## 環境變數

將敏感資訊寫入 `~/.hermes/.env`：

```bash
OPENROUTER_API_KEY=sk-or-...
TELEGRAM_BOT_TOKEN=...
DISCORD_BOT_TOKEN=...
GITHUB_TOKEN=...
```

> ⚠️ **踩坑：** `.env` 的 key 名稱必須與 config.yaml 中定義的 provider 一致，否則 Hermes 讀不到。

## WSL2 環境重點

### Git 認證
WSL 沒有瀏覽器可以互動式 OAuth，必須用 token：

```bash
git config --global credential.helper store
echo "https://你的帳號:你的TOKEN@github.com" > ~/.git-credentials
```

> ⚠️ **踩坑：** 格式必須是 `https://USER:TOKEN@github.com`，少了 USER 會噴 `could not read Username`。

### 網路互通
- WSL2 可以透過 `localhost` 存取 Windows 端的服務（反過來也可以）
- 如果 Windows 跑 Ollama，Hermes 設 `http://localhost:11434/v1` 就能連到

### 更新 Hermes
```bash
hermes update
# 或手動：
cd ~/hermes-agent
git pull
pip install -e .
```

> ⚠️ **踩坑：** 有時 pip 沒刷新 entry point，更新後 `hermes` 指令還是舊版。解法：`pip install -e . --force-reinstall`。

## Docker 部署

### 快速啟動
```bash
docker run -d \
  --name hermes \
  -v ~/.hermes:/root/.hermes \
  -e OPENROUTER_API_KEY=sk-or-... \
  ghcr.io/nousresearch/hermes-agent
```

### Docker Compose
```yaml
version: '3.8'
services:
  hermes:
    image: ghcr.io/nousresearch/hermes-agent
    volumes:
      - ~/.hermes:/root/.hermes
    env_file:
      - ~/.hermes/.env
    restart: unless-stopped
```

> ⚠️ **踩坑：** Docker 版不支援 TUI（`hermes chat`），只能跑 daemon 模式 + 平台通訊。

## 常用指令速查

| 指令 | 用途 |
|------|------|
| `hermes chat` | 啟動互動式對話 |
| `hermes serve` | 背景 daemon 模式（平台通訊生效） |
| `hermes setup` | 重新執行初始化精靈 |
| `hermes config set key value` | 修改設定 |
| `hermes skills list` | 列出已安裝技能 |
| `hermes skills install ./path` | 安裝技能 |
| `hermes memory status` | 檢查記憶 provider 狀態 |
| `hermes cron list` | 列出排程任務 |
| `hermes status` | 查看整體狀態 |
| `hermes update` | 更新到最新版 |
| `hermes web` | 啟動 Web Dashboard |

## 疑難排解

### `hermes: command not found`
```bash
# pip 安裝路徑沒在 PATH 裡
export PATH=$PATH:$HOME/.local/bin
# 或加到 ~/.bashrc
```

### Gateway 無法連線
```bash
hermes gateway status   # 檢查狀態
hermes gateway restart  # 重啟
```

### 模型一直回傳空值
- 檢查 API key 是否有效
- 檢查 model name 是否存在於 provider
- 試 `hermes config set model.default gpt-4o-mini` 換個模型

### Telegram Bot 沒反應
1. 確認 token 正確
2. 確認 Bot 有被加入群組/頻道
3. 檢查是否設定 `TELEGRAM_HOME_CHANNEL`
4. 確認 `hermes serve` 正在執行
