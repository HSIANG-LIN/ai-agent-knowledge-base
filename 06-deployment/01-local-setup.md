---
title: 01-local-setup
date: 2024-05-05
category: Deployment
tags: [local, setup, wsl2, ollama]
---

# 本機開發環境設定

在個人電腦上建立 AI Agent 的開發環境是理解其運作邏輯的第一步。

## 1. WSL2 (Windows Subsystem for Linux) 重點
對於 Windows 使用者，強烈建議使用 WSL2。
- **原因**：大多數 AI 開源工具（Docker, Ollama, Python 依賴）在 Linux 環境下的相容性與效能最佳。
- **建議**：安裝 Ubuntu 22.04 LTS 或更高版本。
- **效能**：確保在 `.wslconfig` 中配置了足夠的 RAM 與 CPU 核心。

## 2. Ollama + Hermes 配置
Ollama 是目前最簡單的本地 LLM 執行方式。

### 安裝步驟
1. 在 WSL2 中執行：`curl -fsSL https://ollama.com/install.sh | sh`
2. 拉取模型：`ollama run hermes-3` (或你指定的模型)
3. **API 連接**：Hermes Agent 會透過 `localhost:11434` 與 Ollama 通訊。

## 3. 開發工具鏈
- **Python 3.10+**：建議使用 `uv` 或 `conda` 管理虛擬環境。
- **Docker Desktop**：安裝 Windows 版並啟用 WSL2 Backend。
- **VS Code**：搭配 Remote - WSL 擴充功能。

## 4. 常見問題 (FAQ)
- **GPU 未被偵測**：確保 Windows 已安裝最新的 NVIDIA Driver，且 WSL2 已安裝 NVIDIA Container Toolkit。
- **Port 衝突**：若 `11434` 被佔用，請檢查是否有其他 LLM 服務正在執行。
