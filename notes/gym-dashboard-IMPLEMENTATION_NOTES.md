# Implementation Notes — Gym Dashboard CC

## 來源 bundle

`~/web_design/system-dashboard_system-dashboard.zip`（Claude Design 產出）

### 用了哪些檔案

| 來源檔 | 用途 |
|--------|------|
| `variants/gym-refined.jsx` | **實際實作依據** — 方向 A 的 React 元件與 CSS |
| `variants/gym-bold.jsx` | **實際實作依據** — 方向 B 的 React 元件與 CSS |
| `canvas-app.jsx` | **設計參考** — 簡報卡 + 交付卡，提供設計判斷 |
| `uploads/HER-ClaudeDesign-collab-brief-2026-04-26.md` | **設計參考** — 協作 brief |
| `uploads/body.html` | **設計參考** — 原站 body 頁基線 |
| `uploads/training.html` | **設計參考** — 原站 training 頁基線 |
| `uploads/sleep.html` | **設計參考** — 原站 sleep 頁基線 |
| `uploads/shared-redesign.css` | **設計參考** — 原站共用 CSS |

### 沒有使用

- `assets/hermes-avatar.jpg` — system dashboard 專用
- `uploads/index.html`, `index-974f5e13.html` — system dashboard baseline
- `uploads/bench_prompt_*.txt` — system dashboard 相關
- `uploads/gemini_go_*.json` — Gemini 設計指引

## 實作方式

### 轉譯策略
同 system dashboard：將 JSX 元件包裝成 standalone HTML，React + Babel CDN 在瀏覽器端渲染。

### CSS 架構
- 方向 A：class root `.gym-r`，暗暖色調 `--bg: #1a1814`
  - 變數：`--orange`, `--green`, `--warm`, `--violet`, `--blue`
  - SVG 圓環做 readiness/recovery/load/body 指標
- 方向 B：class root `.gym-b`，淺色調 `--bg: #f4f1de`
  - 變數：`--orange`, `--green`, `--warm`, `--violet`, `--blue`
  - 雜誌封面排版、大字標題、深色 orders bar 對比

### 方向差異重點
| | A・精煉版 | B・編輯封面版 |
|---|---|---|
| 色調 | 暗（與現站一致） | 淺（雜誌風） |
| 首屏 | 今日指令 + 4 環形指標 | 巨型標題 + 3 gauge 卡 |
| 三艙位 | 卡片式，內含 sparkline + 行動清單 | 長文欄位式，內含長 chart + CTA |
| 脈搏 | 7×2 格子（14 天） | 28 格長條（28 天） |
| 風險佇列 | 表格式 | 編輯式列表（大號序號） |

## 還缺哪些 live data binding

### 優先綁定
1. **Today command**（推/守/恢復）— 需 readiness + sleep 邏輯判斷
2. **Readiness score** — Garmin Connect API
3. **Sleep metrics** — Garmin（時長/深眠/HRV/RHR）
4. **Body composition** — Withings（體重/體脂/肌肉量）
5. **Training load** — `ppl-log.csv`（7d sets / PPL ratio / MEV）

### 次要綁定
6. 風險佇列項目 — 需規則引擎或手動維護
7. 脈搏熱圖 — 需從 ppl-log 讀取訓練類型
8. 頁面導航 — 接通到 training.html / sleep.html / body.html
9. Deep link CTA — 接通到各艙位子頁

## 風險

| 風險 | 嚴重度 | 緩解 |
|------|--------|------|
| 方向 B 淺色偏離現行暗色主題 | 中 | 落地前需做暗色變體或加主題切換 |
| CDN 斷線時頁面空白 | 中 | 定稿後可轉純 HTML |
| 資料硬編碼會過期 | 高 | 選定方向後立即 data-binding |
| 多頁（training/sleep/body）尚未實作 | 中 | 首屏先上，子頁後續迭代 |

## 回滾方法

本目錄 `/projects/gym-dashboard_CC/` 完全隔離：
- 原站 `/projects/gym-dashboard/` 零修改
- 刪除本目錄即完全回滾
- 原始 JSX 設計源檔保存在 `variants-src/`
