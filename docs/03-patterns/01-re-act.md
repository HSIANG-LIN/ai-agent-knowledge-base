---
title: 01-re-act.md
description: ReAct (Reasoning and Acting) 模式詳解
category: Patterns
---

# ReAct 模式 (Reasoning and Acting)

## 概述
ReAct 是一種結合了「推理」(Reasoning) 與「行動」(Acting) 的 AI Agent 設計模式。傳統的 LLM 僅進行推理（Chain-of-Thought），而 ReAct 則讓模型在推理的過程中能夠與外部環境（工具、API、搜尋引擎）進行交互，並根據觀察到的結果調整下一步的推理。

## 核心循環
ReAct 的工作流程遵循一個閉環：
1. **Thought (推理)**: 模型分析當前目標與狀態，決定下一步該做什麼。
2. **Action (行動)**: 模型選擇一個工具並提供執行參數。
3. **Observation (觀察)**: 執行工具後，模型獲取外部環境的回饋結果。
4. **Repeat**: 模型根據觀察結果再次進行 Thought，直到達成目標。

## 程式碼範例 (概念性 Python)

```python
class ReActAgent:
    def __init__(self, tools):
        self.tools = tools
        self.history = []

    def run(self, prompt):
        current_prompt = prompt
        while True:
            # 1. Thought & Action (LLM 生成)
            response = llm.generate(current_prompt + self.get_format_instruction())
            
            if "Final Answer:" in response:
                return response.split("Final Answer:")[-1]

            # 解析 Thought 與 Action
            thought, action, action_input = self.parse(response)
            print(f"Thought: {thought}")
            print(f"Action: {action}({action_input})")

            # 2. Execution (執行工具)
            observation = self.tools[action](action_input)
            print(f"Observation: {observation}")

            # 3. Update Context
            current_prompt += f"\nThought: {thought}\nAction: {action}({action_input})\nObservation: {observation}"

```

## 優化技巧
- **Few-shot Prompting**: 提供高品質的 ReAct 範例，讓模型學會正確的思考與工具呼叫格式。
- **Structured Output**: 強制模型輸出 JSON 格式的 Action，減少解析錯誤。
- **Error Feedback**: 當工具執行失敗時，將錯誤訊息直接回饋給模型，讓其進行自我修正。

## 常見陷阱
- **推理迴圈 (Reasoning Loops)**: 模型陷入重複相同的 Thought/Action，無法跳出。
- **觀察過載 (Observation Overload)**: 工具回傳過多資訊，超過 Context Window 或導致模型分心。
- **幻覺行動 (Hallucinated Actions)**: 模型呼叫了不存在的工具或輸入了錯誤的參數。
