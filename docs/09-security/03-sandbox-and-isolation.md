---
title: Agent 沙箱與隔離
date: 2026-05-06
category: Security
tags: [sandbox, isolation, docker, security]
---

# Agent 沙箱與隔離

## 1. 為什麼需要沙箱？

當 Agent 具備「自主執行能力」（例如執行 Shell 指令或 Python 程式碼）時，它就成為了一個潛在的安全漏洞。透過 **Prompt Injection**，攻擊者可以誘導 Agent 執行超出預期的危險指令，例如：
- 刪除主機檔案系統 (`rm -rf /`)
- 竊取環境變數 (`print(os.environ)`)
- 建立反向 Shell 進行遠端控制

沙箱 (Sandbox) 的目的就是透過「隔離」機制，將 Agent 的活動限制在一個受控、可預期且損害受限的環境中。

## 2. 隔離層級

安全防禦應採取「縱深防禦 (Defense in Depth)」策略，從不同層級進行隔離：

| 層級 | 實作方式 | 說明 |
| :--- | :--- | :--- |
| **命令層級 (Command Level)** | `command_allowlist` | 在指令發送給 Shell 之前，先檢查指令是否在允許清單中。最輕量，但難以處理複雜的 Shell 組合指令。 |
| **容器層級 (Container Level)** | **Docker / Podman** | 將 Agent 的執行環境包裝在容器內。即使 Agent 被控制，攻擊者也只能影響容器內部，無法直接存取宿主機。 |
| **網路層級 (Network Level)** | **Firewall / VPC** | 限制容器內部的網路存取權限，防止 Agent 被用來進行 DDoS 攻擊或向外部發送資料。 |

## 3. Hermes `command_allowlist` 配置

在 `config.yaml` 中，您可以定義 Agent 僅能使用的指令集，這能有效過濾大部分的惡意指令。

```yaml
security:
  command_allowlist:
    - "ls"
    - "cat"
    - "python3"
    - "pip install"
    - "git status"
    # 注意：不應包含 rm, sudo, curl, wget 等高風險指令
```

## 4. Docker Sandbox 實作範例

將 Agent 運行在 Docker 容器中是目前最穩定的方案。

**基本實作邏輯：**
1. **啟動受限容器**：使用 `--network none` (若不需要網路) 或 `--cap-drop=ALL` (移除所有 Linux Capabilities)。
2. **掛載唯讀目錄**：僅掛載必要的專案目錄，且以 `ro` (Read-Only) 模式掛載。
3. **資源限制**：限制 CPU 與記憶體使用量，防止惡意程式碼造成資源耗盡 (DoS)。

**Docker Run 指令範例：**
```bash
docker run -it --rm \
  --network bridge \
  --memory="512m" \
  --cpus="0.5" \
  --cap-drop=ALL \
  -v /home/user/project:/app:ro \
  hermes-agent-sandbox:latest
```

## 5. 容器內的環境隔離

為了讓沙箱更安全，應利用 `shell_init_files` 或環境初始化腳本來強化隔離：

- **限制路徑**：在容器啟動時，透過 `.bashrc` 或 `.zshrc` 設定 `PATH`，確保 Agent 只能執行指定的工具。
- **環境變數清理**：不要將宿主機的所有環境變數都傳入容器，僅注入必要的 `PROJECT_ID` 等資訊。
- **檔案權限**：確保容器內的使用者不是 `root`，而是具有最小權限的普通使用者。
