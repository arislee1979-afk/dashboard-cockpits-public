# Hermes 接手發布報告：Dashboard 新版落地

交付時間：2026-04-26
交付者：Claude Code (CC)
接手者：Hermes
狀態：**待 Hermes review + 發布**

---

## 老大拍板的方向

| Dashboard | 選定方向 | 語言 | 隔離位置 |
|-----------|----------|------|----------|
| System Dashboard | **A・精煉駕駛艙** (mission-strip) | 中文 | `/projects/system_dashboard_CC/index-refined.html` |
| Gym Dashboard | **B・編輯封面版** (editorial cover) | **英文** | `/projects/gym-dashboard_CC/index-bold.html` |

落選方向保留在同目錄（`index-bold.html` / `index-refined.html`），不刪除，供日後參考。

---

## 可直接開啟驗證

- System A：[index-refined.html](http://100.115.92.198:8082/projects/system_dashboard_CC/index-refined.html)
- Gym B (EN)：[index-bold.html](http://100.115.92.198:8082/projects/gym-dashboard_CC/index-bold.html)

需網路連線（Google Fonts + React CDN）。

---

## 原站位置（勿直接覆蓋，先備份）

| Dashboard | 原站路徑 | 備份資料夾 |
|-----------|----------|------------|
| System | `/projects/system_dashboard_Vertex/index.html` | `/projects/system_dashboard_Vertex/backups/` |
| Gym | `/projects/gym-dashboard/index.html` | `/projects/gym-dashboard/backups/` |

---

## Hermes 發布 checklist

### System Dashboard

1. **驗證頁面**：開啟上方連結，確認首屏 10 秒回答：能不能跑 / 哪裡 degraded / queue 是什麼 / 先修哪裡
2. **備份原站**：
   ```bash
   cp /home/arislee1979/projects/system_dashboard_Vertex/index.html \
      /home/arislee1979/projects/system_dashboard_Vertex/backups/index.$(date +%Y%m%d-%H%M%S).pre-cc-redesign.html
   ```
3. **複製 assets**：
   ```bash
   cp /home/arislee1979/projects/system_dashboard_CC/assets/hermes-avatar.jpg \
      /home/arislee1979/projects/system_dashboard_Vertex/assets/
   ```
4. **發布**：
   ```bash
   cp /home/arislee1979/projects/system_dashboard_CC/index-refined.html \
      /home/arislee1979/projects/system_dashboard_Vertex/index.html
   ```

### Gym Dashboard

1. **驗證頁面**：開啟上方連結，確認首屏 10 秒回答：push / hold / recover、risk queue、哪個 bay 需要 deeper dive
2. **注意**：Gym B 是**淺色主題**（cream 底），與現行暗色不同——這是設計意圖
3. **備份原站**：
   ```bash
   cp /home/arislee1979/projects/gym-dashboard/index.html \
      /home/arislee1979/projects/gym-dashboard/backups/index.$(date +%Y%m%d-%H%M%S).pre-cc-redesign.html
   ```
4. **發布**：
   ```bash
   cp /home/arislee1979/projects/gym-dashboard_CC/index-bold.html \
      /home/arislee1979/projects/gym-dashboard/index.html
   ```

---

## 現在是 stub 的部分（data-binding 後續迭代）

### System Dashboard — 需綁定的 live data
| 欄位 | 來源 | 優先 |
|------|------|------|
| Overall state (DEGRADED) | `/system-docs/status/health-latest.md` | P1 |
| Dirty trees count | git 治理快照 | P1 |
| OAuth countdown | token 到期日計算 | P1 |
| Services 9/9 | `systemctl` 狀態 | P2 |
| Intervention queue | `/shared-memory/PROJECTS.md` 或 inbox | P2 |
| Fleet 狀態 | 蝦群 service 狀態 | P3 |
| 輸出串流 | 最新 5 筆蝦群產出 | P3 |

### Gym Dashboard — 需綁定的 live data
| 欄位 | 來源 | 優先 |
|------|------|------|
| Today's order (push/hold/recover) | readiness + sleep 判斷邏輯 | P1 |
| Readiness score | Garmin Connect | P1 |
| Sleep / HRV / RHR | Garmin Connect | P1 |
| Body composition | Withings Body+ | P2 |
| Training volume / PPL ratio | `ppl-log.csv` | P2 |
| Risk queue items | 規則引擎或手動 | P3 |
| 28-day pulse heatmap | `ppl-log.csv` 訓練類型 | P3 |

---

## 技術架構

- 每個頁面是 **self-contained HTML**：React 18.3.1 + Babel CDN 在瀏覽器端渲染 JSX
- CSS 全部透過元件內 `<style>` 自包含，零外部依賴
- System 用 class root `.sys-r`，Gym 用 `.gym-b`
- 後續綁資料時：直接替換 JSX 中的硬編碼文字為 props/state，DOM 結構不動

---

## 回滾方法

| 狀況 | 操作 |
|------|------|
| 還沒發布，想取消 | 刪除 `system_dashboard_CC/` 和 `gym-dashboard_CC/` |
| 已發布，想回滾 | 從 `backups/` 復原原始 `index.html` |

---

## 來源追溯

- Claude Design bundle：`~/web_design/system-dashboard_system-dashboard.zip`
- 設計 brief：`~/vault/inbox/CC-claude-design-handoff-implementation-2026-04-26.md`
- 原始 JSX 設計源：`variants-src/` 子目錄（兩個隔離資料夾內都有留底）
- 完整實作筆記：各目錄內 `IMPLEMENTATION_NOTES.md`

---

## 一句話交付

System A（精煉駕駛艙）+ Gym B（英文編輯封面版）已在隔離資料夾就位，原站零修改。Hermes 跑完上面的 checklist 就可以發布；data-binding 留到下一輪迭代。
