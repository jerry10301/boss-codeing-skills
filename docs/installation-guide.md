# 安裝指南 — 使用前必備工具

使用這些 skills 之前，電腦上需要先裝好以下工具。
本指南用最簡單的步驟說明，每個工具只需要裝一次。

---

## 必須安裝的工具

### 1. Claude Code（主程式）

這是讓你能使用所有 skills 的主體程式。

**下載網址：** https://claude.ai/download

安裝後開啟，把 `.skill` 檔案拖曳進對話框即完成安裝。

---

### 2. Git（存檔 / 讀檔功能的核心）

`/存檔`、`/讀檔`、以及 AI 幫你做完東西後的版本管理，都需要 Git。

#### Windows 安裝

1. 前往 https://git-scm.com/download/win
2. 下載並執行安裝程式
3. **安裝過程全部按「Next」就好，不需要改任何設定**
4. 安裝完成後，打開「命令提示字元」輸入 `git --version`，看到版本號碼就成功了

> 驗證成功畫面範例：`git version 2.45.0.windows.1`

#### Mac 安裝

打開「終端機」（Terminal），輸入以下指令：

```
xcode-select --install
```

彈出視窗後按「安裝」，等待完成即可。

---

## 選擇性安裝（依功能需要）

以下工具不是每次都用到。當 AI 需要它們時，會主動問你是否同意安裝，你說「好」後 AI 會自動處理，**不需要你自己動手**。

但如果你預期會常用到以下功能，可以提前裝好，減少等待時間。

---

### 3. Python（資料處理、Excel 匯出）

**什麼時候需要：** 你要求 AI 幫你做「Excel 表格」、「資料整理」、「批次處理」等功能時。

#### Windows 安裝

1. 前往 https://www.python.org/downloads/
2. 下載最新版本（按大的「Download Python」按鈕）
3. 執行安裝程式時，**務必勾選「Add Python to PATH」**（最下方的選項）
4. 按「Install Now」

> ⚠️ 「Add Python to PATH」這個勾選框非常重要，忘記勾會導致安裝失敗。

#### Mac 安裝

1. 前往 https://www.python.org/downloads/
2. 下載並安裝最新版本

#### 驗證安裝

安裝完成後，打開命令提示字元，輸入：
```
python --version
```
看到版本號碼（如 `Python 3.12.0`）就成功了。

---

### 4. Node.js（網頁工具、自動測試）

**什麼時候需要：** AI 幫你做的是需要「即時互動」的網頁工具、或你的專案有自動測試時。

#### Windows / Mac 安裝

1. 前往 https://nodejs.org/
2. 下載「LTS」版本（頁面左邊那個，比較穩定）
3. 執行安裝程式，全部按「Next」

#### 驗證安裝

```
node --version
```
看到版本號碼（如 `v20.11.0`）就成功了。

---

## 安裝優先順序建議

| 你想要做的事 | 需要安裝 |
|------------|---------|
| 基本使用（討論、整理需求、確認工項）| Claude Code 就夠 |
| 存檔 / 讀檔版本管理 | Claude Code + **Git** |
| 讓 AI 幫你做網頁工具 | Claude Code + Git |
| 讓 AI 幫你處理 Excel / 資料 | Claude Code + Git + **Python** |
| 跑自動測試 | Claude Code + Git + **Node.js** 或 Python（視專案類型）|

**最省事的做法：Claude Code + Git + Python 三個全裝，90% 的情況都夠用。**

---

## 安裝完成確認清單

裝好後可以用以下方式確認：

打開命令提示字元（Windows：按 `Win+R`，輸入 `cmd`，按 Enter），輸入以下指令：

```
git --version
python --version
node --version
```

三個都看到版本號碼就準備好了。

---

## 遇到問題？

安裝時如果遇到問題，把錯誤畫面截圖傳給 AI，說「我裝這個遇到問題」，AI 會引導你解決。
