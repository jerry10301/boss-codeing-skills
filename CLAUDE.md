# Boss Coding Skills — Agent 指引

這個 repo 是一個 **Claude Code skill 組合包**，專為零資訊背景的使用者設計，讓他們能用白話中文指揮 AI 完成程式開發。

---

## 專案結構

```
boss-codeing-skills/
├── CLAUDE.md          ← 你現在讀的這個檔案
├── README.md          ← GitHub 說明頁（給人類使用者看）
├── src/               ← 所有技能的原始碼
│   ├── boss-coding/   ← 組合包（整合以下三者）
│   ├── vibe-coding/   ← 核心開發技能
│   ├── git-save/      ← 存檔技能
│   └── git-load/      ← 讀檔 / 版本還原技能
│       └── SKILL.md   ← 每個技能的唯一原始檔
└── dist/              ← 打包好的 .skill 檔案（供使用者下載）
    ├── boss-coding.skill
    ├── vibe-coding.skill
    ├── git-save.skill
    └── git-load.skill
```

---

## 技能說明

| 技能 | 原始碼 | 功能 |
|------|--------|------|
| `boss-coding` | `src/boss-coding/SKILL.md` | 組合包入口，整合開發 + 存檔 + 讀檔 |
| `vibe-coding` | `src/vibe-coding/SKILL.md` | 使用者描述需求，AI 直接做出來 |
| `git-save` | `src/git-save/SKILL.md` | 白話中文存檔（git commit 封裝） |
| `git-load` | `src/git-load/SKILL.md` | 白話中文版本還原（git checkout 封裝） |

---

## .skill 檔案格式

`.skill` 是副檔名改名的 **ZIP 壓縮檔**，內部結構：

```
<skill-name>.skill   (ZIP)
└── <skill-name>/
    └── SKILL.md
```

### 打包指令

```powershell
# 在 src/ 目錄下執行，<name> 替換為技能名稱
cd src
Compress-Archive -Path "<name>" -DestinationPath "../dist/<name>.skill.zip" -Force
Rename-Item "../dist/<name>.skill.zip" "../dist/<name>.skill"
```

或用 bash（需要 zip）：
```bash
cd src && zip -r "../dist/<name>.skill" "<name>/"
```

---

## 新增技能的步驟

1. 在 `src/` 建立新資料夾：`src/<skill-name>/`
2. 在其中建立 `SKILL.md`，格式如下：

```markdown
---
name: <skill-name>
description: <一行描述，用於 AI 判斷何時觸發>
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

3. 照上方打包指令產出 `dist/<skill-name>.skill`
4. 更新 `README.md` 的技能列表
5. commit

---

## 核心設計原則

這個組合包的所有技能都遵守以下原則，修改時請保持一致：

### 使用者角色定位
- 使用者 = **老闆**，AI = **工具師傅**
- 老闆只說需求和驗收，不碰任何技術操作

### 禁用詞規範
技能內容中絕對不對使用者說的詞彙：

| 禁用 | 替代 |
|------|------|
| git / commit / branch / hash | 存檔、存檔紀錄（不提 git） |
| framework / 框架 | 工具 |
| database | 儲存資料的地方 |
| API | 跟其他服務溝通的方式 |
| terminal / 終端機 | 不提，AI 自己處理 |
| deploy / 部署 | 放到網路上讓別人看得到 |
| debug | 找問題、修問題 |

### 操作禁止事項
- 禁止叫使用者開終端機、複製貼上程式碼、自行安裝任何東西
- 禁止問使用者技術選擇（AI 自己決定）
- 禁止顯示 git hash 或任何英數字代碼給使用者

### 錯誤處理
- 出錯時 AI 自己修，修好後說「我已經修好了」
- 不讓使用者看原始錯誤訊息

---

## 開發注意事項

- `dist/` 內的 `.skill` 是二進位打包檔，不要直接編輯，修改請從 `src/` 的 `SKILL.md` 開始
- `.claude/settings.local.json` 已被 `.gitignore` 排除，不會進入 git
- commit message 請用繁體中文，說明「改了什麼」+「為什麼」
