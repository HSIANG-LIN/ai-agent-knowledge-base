---
title: Tirith 策略引擎配置指南
date: 2026-05-06
category: Security
tags: [tirith, policy-engine, hermes]
---

# Tirith 策略引擎配置指南

## 1. 什麼是 Tirith？

**Tirith** 是 Hermes 內建的安全策略引擎。它的核心作用是在 Agent 決定執行一個「工具調用 (Tool Call)」或「系統指令」之前，先進行攔截與審核。這是一個「零信任 (Zero Trust)」架構的實作，確保 Agent 的行為始終符合預期範圍。

## 2. 配置路徑

所有的 Tirith 配置都位於 `config.yaml` 的 `security.tirith_*` 區段下。

```yaml
security:
  tirith:
    enabled: true
    mode: strict  # 可選: strict (攔截) 或 audit (僅記錄)
    fail_open: false # 如果引擎出錯，是否允許指令執行
    rules:
      - name: "block-destructive-commands"
        action: deny
        pattern: "rm -rf /"
      - name: "restrict-network"
        action: deny
        target: "network_access"
        condition: "not_in_allowlist"
```

## 3. 撰寫策略規則

Tirith 支援三種主要的處理動作：

- **Allow (允許)**：明確允許該行為。
- **Deny (拒絕)**：攔截該行為，並回傳安全錯誤給 Agent，防止危害發生。
- **Audit (審計)**：允許行為發生，但在日誌中詳細記錄該行為，用於後續行為分析。

### 規則定義維度
1. **Target (目標)**：可以針對 `command` (指令)、`file_path` (路徑)、`network` (網路)、`tool` (工具) 等。
2. **Pattern (模式)**：使用 Regex 或簡單字串匹配來定義觸發條件。
3. **Condition (條件)**：複雜邏輯，例如「僅當在特定目錄下時才允許寫入」。

## 4. 實用規則範例

### 範例 A：禁止危險指令
防止 Agent 執行毀滅性的檔案刪除動作。
```yaml
- name: "prevent-rm-rf"
  action: deny
  type: command
  pattern: "^rm\\s+-rf\\s+/"
```

### 範例 B：限制檔案讀寫範圍
確保 Agent 只能在指定的專案目錄內工作，無法讀取系統敏感檔案（如 `/etc/passwd`）。
```yaml
- name: "restrict-filesystem"
  action: deny
  type: file_access
  pattern: "^/(etc|var|root|home/[^/]+/.ssh)/"
```

### 範例 C：限制網路存取
限制 Agent 只能存取特定的 API 網域。
```yaml
- name: "network-allowlist"
  action: deny
  type: network
  pattern: "^(?!api\\.github\\.com|api\\.openai\\.com).*"
```

## 5. 故障處理：`tirith_fail_open` 的作用

在配置中，`security.tirith.fail_open` 是一個關鍵參數：

- **`fail_open: false` (推薦)**：如果 Tirith 引擎因為錯誤、記憶體不足或規則解析失敗而無法判斷，系統會預設**拒絕**該指令。這符合安全優先原則。
- **`fail_open: true`**：如果引擎出錯，系統會**允許**指令繼續執行。這通常用於開發階段，或是在對可用性要求極高、且安全風險可控的場景。
