# 🗺️ Notion 續訪地址互動地圖 Pro 版

本專案提供一個完整的工作流程，實現從 Notion Database 自動擷取續訪地址資料、同步至 Google Sheet，並透過 HTML + Leaflet 地圖進行視覺化展示。此解決方案特別設計用於輔助福音服務追蹤、區域性統計與動態規劃。

---

## 📌 專案功能概述

- 自動從 Notion 讀取標準欄位資料
- 自動快取經緯度，避免重複呼叫地理編碼 API
- 將資料導入 Google Sheet 並提供 JSON Web API
- 使用 Leaflet 製作互動式地圖（支援 Marker Cluster）
- Marker 顏色根據「指派對象」分組標色
- Popup 顯示詳細資訊，並支援 Google Maps 導航與 Notion 超連結
- 支援搜尋與排序功能（如姓名、區域卡、拜訪日期、星等）

---

## 📁 專案結構說明

```
notion-map-project/
├─ index.html         # Leaflet 地圖前端頁面
├─ AppScript.gs       # Google Apps Script 腳本（擷取 Notion 並寫入 Sheet）
└─ README.md          # 本說明文件
```

---

## 📚 Notion Database 欄位需求

### 🔗 串接前準備

1. **建立整合（Integration）：**
   - 前往 [Notion Developer Portal](https://www.notion.so/my-integrations)，建立一個新的 integration。
   - 記錄下你的 Integration Token（用於 Apps Script 串接）。

2. **分享資料庫給 Integration：**
   - 在 Notion 中開啟你要串接的資料庫頁面。
   - 點擊右上角「Share」按鈕 → 將剛剛建立的 Integration 加入分享權限（至少 Viewer 權限）。

3. **取得 Database ID：**
   - 將 Notion 資料庫以「複製連結」方式取得網址，格式如下：
     ```
     https://www.notion.so/yourworkspace/<DATABASE_ID>?<nothing to use>
     ```

---

以下為 Notion Database 建議結構（欄位名稱需精確對應）：

| 欄位名稱        | 類型     | 說明                                 |
|------------------|----------|--------------------------------------|
| Name             | 標題欄位 | 對象姓名                             |
| 地址              | 文字     | 詳細地址（建議包含行政區）           |
| 年紀              | 數字     | 可選填                               |
| 星等              | 選擇     | 🌟 ~ 🌟🌟🌟🌟🌟                         |
| 拜訪日期         | 日期     | 下次預定或最近拜訪日期               |
| Assign to        | 選擇     | 負責拜訪的弟兄姊妹                   |
| 區域卡           | 數字     | 對應地圖或行政區劃分                 |
| 備註              | 多行文字 | 自由補充內容                         |
| Google Map       | 公式     | `https://www.google.com/maps/...`（可選） |
| (lat, lng)       | 文字     | 緯經度，例如：`(25.033, 121.565)`     |
| notion database  | 文字     | 指向該筆資料的 Notion 公開連結       |

---

## ⚙️ Google Apps Script 設定

1. 開啟一份新的 Google Sheet，點選「擴充功能」→「Apps Script」。
2. 貼上 `AppScript.gs` 內容，並設定以下變數：

```javascript
const NOTION_TOKEN = "你的 Notion 整合 Token";
const DATABASE_ID = "你的 Notion Database ID";
```

3. 點擊「執行」按鈕執行 `fetchNotionDataToSheet()`，即會將資料填入 Sheet 中。
4. 點選「部署」→「部署為網頁應用程式」，複製 JSON API URL。

---

## 🌍 HTML 地圖部署說明

1. 將 `index.html` 上傳至 GitHub Pages、Netlify 或任何靜態網站平台。
2. 修改程式中的資料來源：

```javascript
const dataUrl = "https://script.google.com/macros/s/xxx/exec";
```

3. 開啟網頁即可看到完整的互動式 Notion 地圖視覺化。

---

## 🧠 進階功能亮點

- **📍 Marker 分群顯示：** 透過 Leaflet MarkerCluster 套件自動管理 marker 聚合。
- **🎨 顏色分組：** marker 顏色依照「Assign to」自動分配與圖例顯示。
- **🔍 即時搜尋 + 排序功能：** 提供姓名、區域卡篩選與拜訪日期、星等排序。
- **🧭 Google 導航：** popup 中可一鍵開啟 Google Maps，根據經緯度精準導航。
- **📓 Notion 快捷連結：** 透過 `notion database` 欄位快速跳轉原始資料。

---

## 📌 注意事項與建議

- `(lat, lng)` 欄位建議固定格式 `(25.0249774, 121.4933642)`，若無經緯度會導致 marker 顯示失敗。
- 若未提供 `Google Map` 欄位，系統會自動 fallback 至經緯度產生導航連結。
- Apps Script 呼叫 Notion API 受限於速率，建議每日或每週手動或排程更新。
- 若資料量大，請避免過於頻繁刷新地圖（已移除自動 reload）以提升效能。

---

## 👨‍💻 作者資訊

- **開發者：** David Chen  
- **組織：** NTU CAE Group  
- **聯絡方式：** [a0910027898@gmail.com](mailto:a0910027898@gmail.com)  
- **GitHub：** [github.com/NTU-CAE-David](https://github.com/NTU-CAE-David)

---

