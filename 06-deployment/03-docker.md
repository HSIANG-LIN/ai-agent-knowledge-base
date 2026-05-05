---
title: 03-docker-configuration
date: 2026-05-05
category: Deployment
tags: [docker, container, devops]
---

# Docker 技術細節

利用容器化技術確保 Agent 在不同環境間的一致性。

## 1. hermes-agent-docker
我們建議使用官方提供的 Docker 映像檔，以確保環境配置已優化。

## 2. 自訂 Dockerfile
若需加入特定的 Python 依賴或工具，可建立自訂 Dockerfile：

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

## 3. Volume 與 Network 配置
- **Volume (持久化)**：將 `/app/memory` 或 `/app/logs` 掛載到宿主機，避免容器重啟時資料遺失。
- **Network (網路隔離)**：建立專用的 Docker Network，讓 Agent 與其私有資料庫進行通訊，而不對外暴露。
