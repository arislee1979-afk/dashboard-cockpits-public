# Implementation Notes — System Dashboard CC

## 來源 bundle

`~/web_design/system-dashboard_system-dashboard.zip`（Claude Design 產出）

### 用了哪些檔案

| 來源檔 | 用途 |
|--------|------|
| `variants/system-refined.jsx` | **實際實作依據** — 方向 A 的 React 元件與 CSS |
| `variants/system-bold.jsx` | **實際實作依據** — 方向 B 的 React 元件與 CSS |
| `assets/hermes-avatar.jpg` | 直接複製使用 |
| `canvas-app.jsx` | **設計參考** — 四方向簡報卡 + 交付卡，提供設計判斷與原則描述 |
| `design-canvas.jsx` | **設計參考** — 畫布框架，非實作用 |
| `HER-Claude-Cockpits.html` | **設計參考** — 畫布入口 HTML |
| `uploads/HER-ClaudeDesign-collab-brief-2026-04-26.md` | **設計參考** — 協作 brief |
| `uploads/index.html` | **設計參考** — 原站 baseline |
| `uploads/vertex_design_guidance.*` | **設計參考** — Vertex 設計指引 |

### 沒有使用

- `uploads/body.html`, `training.html`, `sleep.html`, `shared-redesign.css` — 這些是 gym dashboard 的基線
- `uploads/bench_prompt_*.txt` — bench prompt 文件
- `uploads/gemini_go_*.json` — Gemini 設計指引
- `variants/_archive/system-refined-v1.jsx` — 舊版，已被 v2 取代

## 實作方式

### 轉譯策略
將 Claude Design 的 JSX 元件包裝成 standalone HTML：
- React 18.3.1 + ReactDOM 從 unpkg CDN 載入
- Babel standalone 在瀏覽器端轉譯 JSX
- 所有 CSS 透過元件內 `<style>` 標籤自包含
- 零建置步驟，直接在瀏覽器開啟即可

### 為什麼用 React CDN 而不是純 HTML
- **忠實度最高**：JSX 元件原封不動嵌入，避免手動轉譯引入的錯誤
- **可驗證性**：老大看到的畫面與 Claude Design 畫布完全一致
- **後續綁資料時成本最低**：已是 React 元件，直接替換硬編碼值為 props/state

### CSS 架構
- 方向 A：class root `.sys-r`，CSS 變數 `--bg`, `--panel`, `--ok`, `--warn`, `--danger`, `--cyan`, `--violet`
- 方向 B：class root `.sys-b`，CSS 變數 `--bg`, `--ink`, `--amber`, `--red`, `--green`, `--cyan`, `--violet`
- 兩個方向的變數名不同（這是設計意圖，非疏忽）

## 還缺哪些 live data binding

### 優先綁定（影響駕駛艙核心判斷）
1. **Overall state** — 目前硬編碼 DEGRADED，需從 health-latest.md 讀取
2. **Dirty trees 數字** — 需 `git status` 或治理快照
3. **OAuth countdown** — 需從 token 到期日動態計算
4. **Services online** — 需 systemctl 狀態掃描
5. **Intervention queue** — 需從 PROJECTS.md 或 inbox 動態讀取

### 次要綁定
6. Fleet 狀態 — 蝦群 service 狀態
7. 輸出串流 — 最新 5 筆蝦群產出
8. 遙測數字 — 儲存/網路/記憶層

## 風險

| 風險 | 嚴重度 | 緩解 |
|------|--------|------|
| CDN 斷線時頁面空白 | 中 | 可在定稿後轉純 HTML 版本 |
| 方向 B 終端機網格在小螢幕過密 | 中 | 背景是 CSS gradient，可依 breakpoint 調暗/簡化 |
| 資料硬編碼會過期 | 高 | 選定方向後立即進入 data-binding 階段 |

## 回滾方法

本目錄 `/projects/system_dashboard_CC/` 是完全隔離的：
- 原站 `/projects/system_dashboard_Vertex/` 零修改
- 刪除本目錄即完全回滾
- 原始 JSX 設計源檔保存在 `variants-src/` 子目錄
