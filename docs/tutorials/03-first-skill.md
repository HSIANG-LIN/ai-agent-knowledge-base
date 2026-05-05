# Hermes Agent 進階教學：編寫你的第一個 Skill

Skill 是 Hermes Agent 的擴充功能，讓 Agent 能夠執行特定任務（如查詢天氣、搜尋檔案）。

## 步驟 1: 建立 Skill 目錄
所有的 Skill 都存放在 `~/.hermes/skills/` 目錄下。請先為你的新 Skill 建立一個資料夾。
```bash
mkdir -p ~/.hermes/skills/my-first-skill
```

## 步驟 2: 撰寫 SKILL.md
在該目錄下建立一個 `SKILL.md` 檔案。這個檔案必須包含 YAML 格式的 Frontmatter 來定義觸發條件。

**範例內容（檔案搜尋 Skill）：**
```markdown
---
name: File Searcher
description: 尋找特定的檔案
triggers:
  - "幫我找檔案"
  - "搜尋檔案"
---

# File Searcher Skill

當使用者提到搜尋檔案時，此 Skill 會被觸發。

## 執行邏輯
使用 `search_files` 工具來尋找檔案。
