# Rehab Task Recommendation Survey

一個前端問卷系統，依使用者作答的條件組合，從 96 種推薦任務中挑出最匹配的一筆。純 HTML/CSS/JS，無後端依賴，雙擊 `index.html` 就能跑。

A front-end questionnaire that maps the user's answer pattern to one of 96 pre-defined recommendation entries. Pure HTML/CSS/JS, no backend, just open `index.html` in a browser.

> ⚠️ 推薦內容已替換為示範文字（`Example exercise A/B/C ...`）。本 repo 僅保留**問卷結構與條件比對邏輯**作為作品集呈現。
> The 96 recommendation bodies are placeholder text. Only the survey flow and rule-matching logic are kept for portfolio use.

---

## 它在做什麼 / What it does

- 問卷分成「事前題（V/AA/N/...）」與「正式題（D/E/F/...）」兩段
- 每題作答後即時更新已收集的條件集合，並依條件樹**跳題、早停**
- 作答結束後將條件集合組合成 key（例：`V_AA_M_D_G_JI`），查表得到對應的推薦 ID（1–96）
- 顯示對應推薦內容（標題、適用對象、訓練建議）
- 全部資料與邏輯都嵌在單一 HTML 檔內，零部署成本

The questionnaire has a screening section (V/AA/N/...) and a detail section (D/E/F/...). Answers are aggregated into a feature set after each question, which drives skip-logic and early-termination. The final set is composed into a lookup key (e.g. `V_AA_M_D_G_JI`) that resolves to one of 96 recommendation IDs.

---

## 技術棧 / Tech stack

- **HTML + 原生 CSS**（CSS variables、grid、flex）
- **原生 JavaScript**（無框架，整支 app 在一個檔內）
- 條件函式表（`hasAnswer(ans, dim)` 之類的 predicate）+ key 組合表

---

## 我怎麼做出這個 / How I built it

這是我**跨領域學網頁開發**做的第一個東西，所以我刻意把流程記下來。

This was my first real web project after switching domains, so I tried to be explicit about the process.

### 1. 研究階段 / Research

- 先理解需求：96 種推薦其實是一張「條件 → 答案」的對照表，本質是查表問題
- 學 HTML/CSS 基本盤、CSS 變數、響應式 grid
- 學 JS 怎麼操作 DOM、怎麼用 `localStorage`/state 物件管理問卷進度
- 看了不少跳題（branch logic）和決策樹資料

### 2. 框架階段 / Scaffolding

- 決定不引入框架，因為這是個一次性問卷，單檔交付最直觀
- 把問題、條件、推薦對照表分成三個資料結構，問卷引擎只負責「跑流程」
- 條件判斷用 predicate function（`'V_AA': (ans) => hasAnswer(ans, 'V') && ans.V`），而不是寫死 if/else，比較好維護

### 3. AI 迭代階段 / AI-assisted iteration

- 寫好骨架後，跟 AI 一題一題對需求、把跳題邏輯實作完
- 改版時遇到 bug 多半是條件預設值沒處理好（例如某題沒答時要算 `false` 而不是 `undefined`），透過跟 AI 對話、用 console.log 試出來的
- 學到的事：**先把資料結構講清楚**，AI 寫的程式碼會穩很多；用「資料 → 規則 → 流程」三層拆解，比一次寫全部容易迭代

---

## 啟動 / Run

```bash
# 沒有任何相依，雙擊或用任何靜態檔伺服器都可
python3 -m http.server 8000
# 然後開 http://localhost:8000
```

---

## 檔案 / Files

```
.
├── index.html         # 問卷 UI + 流程引擎 + 96 筆推薦（內容為 placeholder）
├── recommendations.txt # 推薦對照表的另存版本（同樣是 placeholder）
└── README.md
```

---

## 後續可以做的事 / Next steps

- 把推薦內容從 HTML 拉出來，改成 fetch 一個 JSON
- 加上作答結果匯出（PDF / CSV）
- 把跳題邏輯改寫成更通用的 DSL，讓非工程師也能編輯

---

## License

MIT — see [LICENSE](LICENSE).
