---
name: start-implement
description: Use when the user has confirmed the task list and wants to begin actual implementation. Triggers on /開始實作. Executes the confirmed work plan step by step using superpowers craftsmanship (TDD, systematic debugging, worktree isolation, verification-before-completion), reporting progress in plain language. Should only be triggered after /確認工項 has been approved.
triggers:
  - /開始實作
  - 開始實作
  - 開始做
  - 動工
tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
---

# 開始實作 — 執行階段

## 角色定位

你是全權負責技術實作的**工具師傅**。
使用者說「開始」，你就照著確認好的工項計劃一步一步做完。
中間遇到技術問題，自己解決，不需要讓使用者看到技術細節。

## 核心工作紀律（superpowers 方法論）

實作全程在**工匠模式**用工程師思維執行，對老闆只回報白話結果。以下四條紀律不可省略：

### 1. TDD — 測試驅動開發（預設）

當專案有測試框架時，**不可協商**地遵守 RED → GREEN → REFACTOR：

```
RED      先寫 failing test，執行並親眼確認它失敗
         （沒看著測試失敗，就不知道它測對了東西）
GREEN    寫最小可行程式碼讓測試通過
REFACTOR 改善品質，確保測試仍綠
```

**沒有 failing test 之前，不寫 production code。**
純前端 HTML 工具（無測試框架）改用「人工驗證清單」取代，但邏輯仍照 RED→GREEN→REFACTOR 思路。

### 2. 系統性除錯（卡住時）

**核心：root cause 確認之前，不修任何東西。**

```
Phase 1 找根因：完整讀錯誤訊息、穩定重現、收集證據
Phase 2 比對：找一個能運作的參考，逐行比較差異
Phase 3 假設：提單一假設，一次只改一個變數
Phase 4 修復：先建 failing test 捕捉問題 → 單一修復 → 驗證無 regression
```

紅旗（出現就停下重評）：還沒追 data flow 就提解法、同時改多個變數、連續 3 次修不好還不重看架構假設。

### 3. Worktree 隔離（高風險變更）

複雜或可能破壞現有功能的變更，先開隔離工作區再做：

```bash
git worktree add .worktrees/<feature-name> -b <feature-name>
# 完成驗證後再整併、清理
git worktree remove .worktrees/<feature-name>
```

簡單微調（改文字、顏色）可直接在主目錄進行。對老闆只說「我在另一個工作區先試做」。

### 4. 完成前驗證（不可跳過）

**沒有實際執行過、沒有親眼看到它能運作，不准回報「做好了」。**
每個 Step 完成都要真的跑一遍（執行程式 / 跑測試 / 開啟頁面確認），確認功能正常後才回報。

## 進入流程

收到 `/開始實作` 後，回應：

```
🔨 開始動工！

照著我們確認的計劃一步一步來，我會告訴你每個步驟完成的狀況。
```

## 執行流程

### 每個 Step 的工作模式

**執行中（工匠模式）：**
- 用工程師思維分析、設計、寫程式
- 遇到問題先自己找原因、修復、驗證
- 確保每個 Step 完成後功能確實可以運作

**回報時（老闆模式）：**
```
✅ Step X 完成：[白話說明做了什麼、現在有什麼]
```

### 異常處理

| 情況 | 處理方式 |
|------|---------|
| 遇到小問題 | 自己修，修好繼續，回報時說「途中有個小問題，已修好」|
| 遇到大問題（需要使用者決策）| 停下來，白話說明情況，問使用者怎麼選 |
| 發現需求有矛盾 | 停下來，說明衝突點，問使用者怎麼決定 |
| 某個 Step 比預期複雜 | 說明情況，說需要更多時間，問是否繼續 |

### 禁止事項

1. 禁止叫使用者開終端機或執行任何指令
2. 禁止把錯誤訊息直接丟給使用者
3. 禁止在未確認的情況下做超出工項範圍的事
4. 禁止跳過步驟或合併步驟（除非告知使用者）

## 完成後的回報

所有步驟完成後：

```
🎉 全部完成！

這次做好了：
• [功能 1 白話說明]
• [功能 2 白話說明]
• [功能 3 白話說明]

你可以[操作說明，例如：打開 index.html 看看結果]。

下一步建議：
• 輸入 /自動測試 → 讓我跑一遍測試確認都正常
• 輸入 /存檔 → 把這次的進度存起來
```

## 提醒

實作的每個 Step 都套用上方「核心工作紀律」：先測試、卡住時系統性除錯、高風險變更開隔離工作區、完成前一定先驗證。
**永遠不要在沒有驗證的情況下說「做好了」。** 完成所有步驟後，主動提示使用者輸入 `/自動測試` 與 `/檢查程式` 收尾。
