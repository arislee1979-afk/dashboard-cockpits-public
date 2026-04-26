# Aris 每日訓練駕駛艙 — CC 實作版

接手自 Claude Design handoff bundle `system-dashboard_system-dashboard.zip`
產出時間：2026-04-26

## 兩個方向

| 檔案 | 方向 | 設計語氣 |
|------|------|----------|
| `index-refined.html` | A・精煉每日駕駛艙 | 暗暖色調。今日指令在首屏，三艙位（訓練/恢復/身體）物件化，附風險佇列 + 14 天脈搏熱圖。 |
| `index-bold.html` | B・編輯封面版 | 淺色雜誌封面風。巨型襯線標題、三道任務卡、三個長文艙位、28 天脈搏條。質感最重、像產品。 |

## 如何開啟

透過 WebDAV（推薦）：
- [A・精煉版](http://100.115.92.198:8082/projects/gym-dashboard_CC/index-refined.html)
- [B・編輯封面版](http://100.115.92.198:8082/projects/gym-dashboard_CC/index-bold.html)

或直接用瀏覽器開啟本地檔案。需要網路連線（Google Fonts + React CDN）。

## 哪些仍是 stub

- 所有數據是 **硬編碼佔位值**（2026-04-24 快照）
- 尚未綁定 live data sources：
  - Garmin Connect（readiness / HRV / sleep / RHR）
  - Withings Body+（體重 / 體脂 / 肌肉量）
  - `ppl-log.csv`（訓練組數 / PPL 比例 / MEV）
- 頁面導航（今日/訓練/恢復/身體/歷史）目前無實際跳轉
- 艙位 CTA 的 deep link 尚未接通
- 脈搏熱圖是靜態色塊

## 如何之後發布

1. 老大選定 A 或 B
2. 將選定版本的 CSS 變數提升為共用 `tokens.css`
3. 綁定 live data（Garmin / Withings / ppl-log）
4. 複製到 `/projects/gym-dashboard/` 覆蓋原站
5. 注意：方向 B 是淺色，與現行暗色主題不同，若要保持暗色需額外處理

## 回滾

原站完全未被修改：
- 原站位置：`/projects/gym-dashboard/`
- 本目錄是完全隔離的新實作，刪除即回滾

## 資料夾結構

```
gym-dashboard_CC/
├── index-refined.html       ← 方向 A
├── index-bold.html          ← 方向 B
├── assets/                  ← （目前為空，預留 gym 圖片用）
├── variants-src/            ← 原始 JSX 設計源檔（留底）
│   ├── gym-refined.jsx
│   └── gym-bold.jsx
├── README.md
└── IMPLEMENTATION_NOTES.md
```
