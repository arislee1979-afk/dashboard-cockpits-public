# Hermes System Dashboard — CC 實作版

接手自 Claude Design handoff bundle `system-dashboard_system-dashboard.zip`
產出時間：2026-04-26

## 兩個方向

| 檔案 | 方向 | 設計語氣 |
|------|------|----------|
| `index-refined.html` | A・精煉駕駛艙 | mission-strip 首屏：DEGRADED 狀態 + 介入佇列 + OAuth 倒數。暗色、保留現有語彙。 |
| `index-bold.html` | B・指揮中心版 | NORAD 終端機網格：三欄（Operator / 主信號 Billboard / 遙測）。等寬字型、LED 狀態燈、戰備感。 |

## 如何開啟

透過 WebDAV（推薦）：
- [A・精煉版](http://100.115.92.198:8082/projects/system_dashboard_CC/index-refined.html)
- [B・指揮中心版](http://100.115.92.198:8082/projects/system_dashboard_CC/index-bold.html)

或透過 code-server 在本地開啟：
```
# 直接用瀏覽器打開
open /home/arislee1979/projects/system_dashboard_CC/index-refined.html
```

需要網路連線（Google Fonts + React CDN）。

## 哪些仍是 stub

- 所有數據目前是 **硬編碼佔位值**（2026-04-24 快照）
- 尚未綁定 live data sources：
  - `/system-docs/status/health-latest.md`
  - `/shared-memory/PROJECTS.md`
  - `/cc-system/results/*.json`
- 佇列列/指令卡的 CTA 目前無實際跳轉
- fleet pulse 是靜態高度條，非即時

## 如何之後發布

1. 老大選定 A 或 B
2. 將選定版本的 CSS 變數提升為 `tokens.css`
3. 綁定 live data（替換硬編碼文字為資料模板）
4. 複製到 `/projects/system_dashboard_Vertex/` 覆蓋原站
5. 備份原站到 `backups/` 再覆蓋

## 回滾

原站完全未被修改：
- 原站位置：`/projects/system_dashboard_Vertex/index.html`
- 原站備份：`/projects/system_dashboard_Vertex/backups/`
- 本目錄是完全隔離的新實作，刪除即回滾

## 資料夾結構

```
system_dashboard_CC/
├── index-refined.html       ← 方向 A
├── index-bold.html          ← 方向 B
├── assets/
│   └── hermes-avatar.jpg    ← Hermes 頭像
├── variants-src/            ← 原始 JSX 設計源檔（留底）
│   ├── system-refined.jsx
│   └── system-bold.jsx
├── README.md
└── IMPLEMENTATION_NOTES.md
```
