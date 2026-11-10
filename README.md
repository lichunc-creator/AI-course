# AI 融入課程設計小幫手（TCK／TPCK）

給**沒有 AI 背景的大專教師**使用的網頁工具：上傳教學計畫表 → 選單元 → 自動產出以 TCK／TPCK 為基礎的 AI 融入課程規劃，包含評量設計。

**立即試用：** https://claude.ai/artifact/5utxd4HRjakveAYVPxJpgq
（需先登入 Claude 帳號，免費版即可；第一次分析時請按「允許」）

![首頁](screenshots/01_home.png)

---

## 功能

1. **上傳教學計畫**：支援 PDF、Word（.docx）、文字檔，一次最多 5 個；也可直接貼上文字。檔案只在瀏覽器中讀取。
2. **確認課程與選單元**：AI 整理科目、課程目標、現行評分，並把課程大綱拆成單元。
3. **產生規劃**：
   - **TCK 五格**：內容知識 → 傳統呈現的困難 → 科技呈現 → AI 教學機器人 → 使用規範
   - **TPCK 五步驟**（課程重塑步驟）：確認內容知識 → 確認目標 → 拆解步驟 → 比較與批判 → AI 應用與評量
   - **課堂流程**、**評量設計與評分規準**（必含「AI 使用與查證」向度）
   - **教學機器人設定指令**、**學生任務單**、實施提醒
   - 「複製全部內容」可貼到教案或簡報

> `screenshots/` 中的截圖使用示範資料，實際產出內容會由 Claude 依各自的教學計畫生成。

---

## 檔案結構

整個工具只有一個檔案：`index.html`（HTML＋CSS＋JavaScript，無需建置）。

| 區塊 | 位置 | 說明 |
|---|---|---|
| 樣式／配色 | `<style>` 開頭的 `:root{...}` | `--navy` 主色、`--t1`~`--t5` TCK 卡片色、`--c1`~`--c5` TPCK 卡片色 |
| 頁面文字 | `<body>` 內的 `<section>` | 標題、步驟說明、下拉選單選項（課堂時間、AI 工具） |
| 五格標題 | `TCK_HEADS`、`TPCK_HEADS` | 改這兩個陣列即可更改卡片標題 |
| 課程分析提示詞 | `$("analyze").onclick` 內的 `askJSON(...)` | 控制如何從教學計畫拆出單元 |
| 規劃產生提示詞 | `generate()` 內的 `askJSON(...)` | **最主要的設計邏輯**：設計原則、每欄字數、輸出欄位 |
| 結果呈現 | `renderPlan()` | 投影片版面、表格；新增欄位時要同時改提示詞與這裡 |
| 複製內容 | `planText()` | 「複製全部內容」的文字格式 |
| 範例資料 | `const SAMPLE` | 「用範例試試」按鈕放入的教學計畫文字 |

### 常見修改範例

- **改成其他領域**（例如幼保、資訊）：修改 `generate()` 提示詞中的設計原則與「正確專業術語」描述，並替換 `SAMPLE`。
- **增加產出項目**（例如「延伸閱讀」）：在提示詞的 JSON 格式加一個欄位 → 在 `renderPlan()` 加一個 `<div class="card">` 顯示 → 在 `planText()` 加一行。
- **改評量規則**：修改提示詞最後一段（`weights 合計 100%；rubric 3–4 個向度…`）。
- **改配色**：只改 `:root` 的 CSS 變數。

---

## 執行方式與限制（重要）

AI 功能使用 claude.ai 的 artifact 執行環境（`claude.use("sample")`），**只有以 claude.ai artifact 開啟時才能呼叫 Claude**；使用量計入每位使用者自己的 Claude 方案。

- 直接用瀏覽器開啟 `index.html` 或放在 GitHub Pages：上傳與讀檔可用，但「分析課程」會顯示無法呼叫 Claude。
- **想發布自己修改的版本**：在 Claude 對話中提供修改後的 `index.html`，請 Claude 發布成 artifact，並宣告 `sample` 能力；再到該頁面按「分享」。
- **想在 claude.ai 以外獨立運作**：需把 `askJSON()` 改為呼叫 Anthropic API，並透過後端（例如 Cloudflare Workers）保管 API 金鑰，**不要把金鑰寫在網頁裡**。

外部函式庫（皆由 CDN 載入）：[pdf.js](https://github.com/mozilla/pdf.js) 3.11.174、[mammoth.js](https://github.com/mwilliamson/mammoth.js) 1.6.0、Noto Sans TC（Google Fonts）。

---

## 使用提醒

AI 產出內容僅供教學設計參考，請教師依專業判斷審閱；醫療數值、藥物與處置相關內容須以教科書或臨床指引為準。

## 授權

MIT License，歡迎修改與再利用，請保留原作者標示。
