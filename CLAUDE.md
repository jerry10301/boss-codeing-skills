# Boss Coding Skills — Agent 指引

這個 repo 是一個 **Claude Code skill 組合包**，專為零資訊背景的使用者設計，讓他們能用白話中文指揮 AI 完成程式開發。

---

## 專案結構

```
boss-codeing-skills/
├── CLAUDE.md          ← 你現在讀的這個檔案
├── README.md          ← GitHub 說明頁（給人類使用者看）
├── docs/              ← 給人類使用者的文件
│   └── installation-guide.md  ← 安裝指南（Claude Code / Git / Python / Node.js）
├── src/               ← 所有技能的原始碼，每個技能一個資料夾、內含唯一的 SKILL.md
│   ├── boss-coding/         ← 組合包入口（整合開發 + 存檔 + 讀檔，並提供兩條路徑）
│   ├── vibe-coding/         ← 快速路徑：描述需求 AI 直接做（被 boss-coding 引用）
│   ├── git-save/            ← 存檔技能
│   ├── git-load/            ← 讀檔 / 版本還原技能（永不丟資料）
│   ├── discuss/             ← 結構化路徑①：自由討論、腦力激盪
│   ├── gather-requirements/ ← 結構化路徑②：收斂成需求清單
│   ├── confirm-tasks/       ← 結構化路徑③：產出工項計劃，HARD GATE 等確認
│   ├── start-implement/     ← 結構化路徑④：照計劃實作（TDD / 系統性除錯 / 完成前驗證）
│   ├── auto-test/           ← 結構化路徑⑤：執行自動測試驗證
│   └── code-check/          ← 結構化路徑⑥：code review，分級回報
└── dist/              ← 打包好的 .skill 檔案（供使用者下載，共 10 個，與 src 一一對應）
```

---

## 技能說明

| 技能 | 原始碼 | 功能 |
|------|--------|------|
| `boss-coding` | `src/boss-coding/SKILL.md` | 組合包入口，整合開發 + 存檔 + 讀檔，提供快速與結構化兩條路徑 |
| `vibe-coding` | `src/vibe-coding/SKILL.md` | 快速路徑：使用者描述需求，AI 直接做出來 |
| `git-save` | `src/git-save/SKILL.md` | 白話中文存檔（git commit 封裝） |
| `git-load` | `src/git-load/SKILL.md` | 白話中文版本還原（永不丟資料：先備份再以新存檔取回舊版） |
| `discuss` | `src/discuss/SKILL.md` | 自由討論、腦力激盪（蘇格拉底式引導，不實作） |
| `gather-requirements` | `src/gather-requirements/SKILL.md` | 收斂討論成結構化需求清單 |
| `confirm-tasks` | `src/confirm-tasks/SKILL.md` | 產出工項計劃與驗證方式，HARD GATE 等使用者確認 |
| `start-implement` | `src/start-implement/SKILL.md` | 照確認的計劃實作（TDD / 系統性除錯 / worktree / 完成前驗證） |
| `auto-test` | `src/auto-test/SKILL.md` | 偵測測試框架、執行測試、白話回報 |
| `code-check` | `src/code-check/SKILL.md` | code review，依嚴重度（紅/黃/綠）分級回報 |

### superpowers 階段 ↔ 技能對應

結構化路徑就是 superpowers 七階段工作流的白話化封裝，各階段之間有閘門：

```
/討論 → /整理需求 → /確認工項 →（HARD GATE）→ /開始實作 → /自動測試 → /檢查程式 → 存檔
brainstorm  收斂        plan+確認                TDD實作      verify      review     完成
```

| superpowers 階段 | 本專案技能 |
|------------------|-----------|
| Brainstorming（蘇格拉底式探索） | `discuss` |
| Planning（收斂想法） | `gather-requirements` |
| Planning（拆任務、明確路徑、驗證步驟） | `confirm-tasks` |
| Worktree + TDD + 實作 | `start-implement` |
| Verification | `auto-test` |
| Code Review（嚴重度分級） | `code-check` |
| 完成 / 存檔 | `git-save` |

**閘門原則**：`confirm-tasks` 產出計劃後，使用者未明確確認前不得進入 `start-implement`；
`start-implement` 未完成前驗證前不得回報「做好了」。

---

## 開發方法論（superpowers 架構）

**所有對這個 repo 的修改，都必須遵守以下工程標準。**
參考來源：[https://github.com/obra/superpowers](https://github.com/obra/superpowers)

### 雙模式溝通原則

本專案的技能設計遵守嚴格的雙模式溝通架構：

| 模式 | 時機 | 語言 |
|------|------|------|
| **工匠模式** | 分析、設計、實作、除錯過程 | 工程師語言，技術精確 |
| **回報模式** | 向使用者說明進度或結果時 | 非技術語言，老闆聽得懂的白話 |

修改任何技能的 SKILL.md 時，必須維持這個雙模式架構。

### TDD — 測試驅動開發

```
RED    → 先寫 failing test，執行確認它失敗
GREEN  → 最小程式碼讓測試通過
REFACTOR → 改善品質，維持綠燈
```

**不可協商：沒有 failing test，不寫 production code。**

對 SKILL.md 的修改，等同於「寫技能測試再修改技能」：先在 `tests/` 描述預期行為（壓力情境），再修改技能內容，再驗證。

### 系統性除錯

```
Phase 1 — Root Cause：仔細讀錯誤，確認可重現，收集 evidence
Phase 2 — Pattern：找可運作的參考，逐行比較差異
Phase 3 — Hypothesis：單一假設，一次改一個變數
Phase 4 — Fix：建 failing test → 單一修復 → 驗證無 regression
```

**禁止在 root cause 確認前修任何東西。**

### Git Worktree 隔離

複雜功能或可能破壞現有 skill 的變更，使用 worktree：

```bash
git worktree add .worktrees/<feature-name> -b <feature-name>
# 完成後
git worktree remove .worktrees/<feature-name>
```

`.worktrees/` 已加入 `.gitignore`。

### 變更標準

- **不做推測性修改**：每個改動必須對應一個可驗證的問題
- **設計先於實作**：HARD GATE — 設計確認前不寫程式碼
- **每個 PR 對應單一問題**：不捆綁不相關的修改

---

## .skill 檔案格式

`.skill` 是副檔名改名的 **ZIP 壓縮檔**，內部結構：

```
<skill-name>.skill   (ZIP)
└── <skill-name>/
    └── SKILL.md
```

> ⚠️ **路徑分隔符必須是正斜線 `/`**。ZIP 規格規定條目路徑用 `/`；用反斜線 `\` 的條目
> 不符規格，嚴格的解壓器或非 Windows 環境可能把 `<name>\SKILL.md` 當成一個扁平檔名而裝不起來。

### 標準打包法（跨平台、保證正斜線）— Python

這是**唯一建議**的打包方式，在任何平台都產出正斜線條目：

```bash
python - "<name>" <<'PY'
import sys, zipfile, pathlib
name = sys.argv[1]
src = pathlib.Path("src") / name / "SKILL.md"
out = pathlib.Path("dist") / f"{name}.skill"
with zipfile.ZipFile(out, "w", zipfile.ZIP_DEFLATED) as z:
    z.write(src, f"{name}/SKILL.md")   # 永遠用正斜線
print("packed", out)
PY
```

### 替代法（bash，需要 zip）

```bash
cd src && zip -r "../dist/<name>.skill" "<name>/"
```

> ❌ **不要用** PowerShell `Compress-Archive`：在 Windows 上它會產出反斜線條目（`<name>\SKILL.md`），
> 不符 ZIP 規格。若已用它打包，請改用上方 Python 法重打。

---

## 新增技能的步驟

1. **建立測試情境**（類比 RED）：在 `docs/` 或 `tests/` 描述這個技能要解決的壓力情境
2. 在 `src/<skill-name>/` 建立 `SKILL.md`，格式如下：

```markdown
---
name: <skill-name>
description: Use when... <觸發情境，最多 1024 字元>
triggers:
  - /觸發詞
  - 觸發詞
tools:
  - Read
  - Write
  - Bash
  - Edit
---

# 技能內容
...
```

3. 驗證技能行為符合預期（類比 GREEN）
4. 打包為 `dist/<skill-name>.skill`
5. 更新 `README.md` 的技能列表
6. Commit（訊息用繁體中文）

---

## 核心設計原則

本組合包的所有技能都圍繞一個核心概念：**使用者 = 老闆，AI = 工具師傅**。

### 老闆師傅模型

- 老闆只說需求和驗收，不碰任何技術操作
- 師傅負責設計、實作、除錯、全部技術決策
- 師傅內部用工程師語言工作，對老闆回報時說白話

### 對老闆的禁用詞規範

修改技能時，確保以下詞彙不出現在給使用者看的回覆中：

| 禁用 | 替代 |
|------|------|
| git / commit / branch / hash | 存檔、存檔紀錄 |
| worktree / branch | 另一個工作區（不主動提及）|
| framework / 框架 | 工具 |
| database | 儲存資料的地方 |
| API | 跟其他服務溝通的方式 |
| terminal / 終端機 | （不提，AI 自己處理） |
| deploy | 放到網路上讓別人看得到 |
| CSS | 外觀設定 |
| debug | 找問題、修問題 |
| stack trace / error | 發現一個小問題（AI 自己修好再說）|
| regression | （不提）|

---

## 開發注意事項

- `dist/` 內的 `.skill` 是二進位打包檔，**不要直接編輯**，修改從 `src/` 的 `SKILL.md` 開始
- `.claude/settings.local.json` 已被 `.gitignore` 排除
- `.worktrees/` 已被 `.gitignore` 排除
- Commit message 請用繁體中文，說明「改了什麼」+「為什麼」
- 技能描述（`description:` 欄位）開頭用「Use when...」格式，讓 AI 更容易判斷觸發時機
