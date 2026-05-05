---
title: 02-vps-deployment
date: 2026-05-05
category: Deployment
tags: [vps, docker, production]
---

# VPS 部署指南

將 Agent 從本機遷移到雲端伺服器（VPS）是實現 24/7 運行的關鍵。

## 1. 選擇 VPS 供應商
建議選擇具備較好網路延遲與硬體資源的供應商（如 DigitalOcean, Hetzner, AWS）。
- **核心建議**：若需本地 LLM 運行，請選擇帶有 GPU 的實例；若使用 OpenAI/Claude API，則僅需標準 CPU 實例即可。

## 2. 使用 Docker Compose 部署
使用 Docker Compose 可以一鍵啟動 Agent 及其依賴服務（如 Redis, Postgres）。

```yaml
version: '3.8'
services:
  hermes-agent:
    image: hermes-agent:latest
    ports:
      - "8000:8000"
    environment:
      - LLM_API_KEY=${API_KEY}
    volumes:
      - ./data:/app/data
    restart: always
```

## 3. 反向代理 (Reverse Proxy)
使用 **Nginx** 或 **Caddy** 來處理 HTTPS 流量。
- **SSL 憑證**：使用 Let's Encrypt 自動管理。
- **安全性**：僅暴露必要的 Port（如 80, 443），其餘使用防火牆（UFW）封鎖。

## 4. 監控與日誌
- **Prometheus + Grafana**：監控系統資源（CPU/RAM）與 API 響應延遲。
- **Log Rotation**：確保日誌不會撐爆硬碟空間。
