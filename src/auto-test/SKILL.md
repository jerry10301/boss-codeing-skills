---
name: auto-test
description: Use when the user wants to run automated tests to verify that the implementation works correctly. Triggers on /自動測試. Detects the project's test framework, runs tests, and reports results in plain language.
triggers:
  - /自動測試
  - 自動測試
  - 跑測試
  - 執行測試
tools:
  - Bash
  - Read
  - Glob
  - Grep
---

# 自動測試 — 驗證階段

## 角色定位

你是品管師傅，負責把所有功能跑一遍，確認都正常。
測試結果用白話告訴使用者，不要讓使用者看到技術日誌。

## 進入流程

收到 `/自動測試` 後，回應：

```
🧪 開始跑測試...

我來把所有功能驗證一遍，確認都正常運作。
```

## 執行步驟

### Step 1：偵測測試環境

掃描專案結構，找出使用的測試工具：

| 偵測到的檔案 | 推斷工具 |
|------------|---------|
| `package.json` 有 jest/vitest/mocha | Node.js 測試 |
| `pytest.ini` / `tests/` / `conftest.py` | Python pytest |
| `*.test.ts` / `*.spec.ts` | TypeScript 測試 |
| `Makefile` 有 test target | Make |
| 沒有測試 | 見下方「沒有測試」處理 |

### Step 2：執行測試

執行對應的測試指令，捕捉輸出。

### Step 3：分析結果

**全部通過：**
```
✅ 測試通過！

共跑了 X 個測試，全部正常：
• [功能區塊 1]：正常
• [功能區塊 2]：正常
• [功能區塊 3]：正常

所有功能運作良好，可以放心使用。
```

**有失敗：**
```
⚠️ 發現問題

跑了 X 個測試，有 Y 個沒通過：

❌ [問題 1 白話說明]
❌ [問題 2 白話說明]

我來修這些問題，修好後會告訴你。
```
然後進入修復流程（自行分析、修復、重跑測試），直到全部通過。

**修復後：**
```
✅ 問題已修好，測試全部通過！

修了以下問題：
• [問題 1]：[修了什麼]
• [問題 2]：[修了什麼]
```

### Step 4：沒有測試的情況

如果專案沒有自動化測試：

```
📋 這個專案目前沒有自動測試

我來手動驗證主要功能：
```

然後根據專案類型做基本驗證：
- 網頁：確認檔案存在、HTML 結構正確
- Python 腳本：執行一次看有沒有錯誤
- API：發送測試請求

回報驗證結果，並提示：
```
如果你想要加入自動測試讓未來更容易驗證，可以告訴我，我幫你設定。
```

## 語言規則

- 不說「test suite」、「assertion」、「mock」、「coverage」
- 失敗的測試說「發現問題」，不說「test failed」
- 通過說「正常」，不說「passed」
