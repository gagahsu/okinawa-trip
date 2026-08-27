# 沖繩四天三夜 · 親子行程

9/5–9/8 台南 → 沖繩（那霸／名護／恩納／瀨長島）→ 台南，四天三夜親子自駕行程頁面。

🔗 **線上網址：** https://okinawa-trip-cbaa6.web.app

## 這是什麼

一份單頁行程網站，一開始是把所有行程寫死在 HTML 裡的靜態 MVP；現在已經加上一些「正式版」才有的功能，讓四個人可以真的靠這個網站對行程、對進度、傳訊息，而不只是各自看各自的畫面。

## 功能

- **逐日行程時間軸**：4 天行程，每個時間點都有圖示、備註、Q 版路線地圖
- **📅 匯出行事曆**：每個行程項目可以匯出成 `.ics`，一鍵加進 iPhone / Google 行事曆（時間會自動換算成沖繩時區 UTC+9）；也有「匯出整趟行程」一次匯入全部
- **🧭 一鍵導航**：每個地點的「開啟導航」按鈕是真正的 Google Maps 連結，點下去直接開導航 App
- **🌦️ 即時天氣**：頁面上方自動顯示沖繩未來預報（Open-Meteo，免金鑰）
- **✅ 多人同步打勾**：每個行程項目可以勾選「已完成」，四個人手機看到的狀態即時同步（Firebase Firestore）
- **💬 留言板**：即時留言，例如「我們已經到囉」，大家都看得到
- **📴 PWA 離線支援**：可以加到手機主畫面，離線也能看行程（Service Worker 快取）
- **⚠️ 注意事項 / 🚙 自駕須知 / 🧳 打包清單**：行前提醒整理
- **🛠️ 正式版規劃**：記錄目前哪些功能做了、哪些還沒做，以及為什麼

## 技術棧

純前端，沒有建置工具：一份 `okinawa-trip.html`（HTML + CSS + 原生 JavaScript），搭配 Firebase（Firestore 存打勾/留言資料、Firebase Hosting 託管網站）。

```
okinawa-trip.html   行程主頁面（結構、樣式、邏輯都在這一份檔案裡）
manifest.json        PWA manifest
sw.js                Service Worker（離線快取）
icon.svg             App 圖示
firebase.json         Firebase Hosting 設定
.firebaserc           Firebase 專案對應設定
```

## 開發 / 部署

本機預覽（純靜態檔案，任何 http server 都可以）：

```bash
python -m http.server 8000
# 開瀏覽器 http://localhost:8000/okinawa-trip.html
```

部署到 Firebase Hosting：

```bash
npm install firebase-tools
npx firebase login
npx firebase deploy --only hosting
```

## Firebase 設定

`okinawa-trip.html` 裡的 `FIREBASE_CONFIG` 已指向專案 `okinawa-trip-cbaa6`（Firestore + Hosting）。這裡的 `apiKey` 是 Firebase Web SDK 的公開設定值，本來就設計成可以放在前端程式碼裡，真正的存取控制要靠 Firestore 安全規則（而不是隱藏這組 key）。

資料結構：

- `trips/okinawa-2026-09`：`completed` 欄位存每個行程項目的打勾狀態
- `trips/okinawa-2026-09/comments`：留言板訊息（`author`、`text`、`ts`）

## 還沒做的部分

詳見網站內的「🛠️ 正式版規劃」分頁，簡單說：

- 行程資料目前仍寫死在 HTML 裡，還沒接資料庫/Google Sheets，改行程還是要改程式碼
- 沒有即時路況／車程時間試算（需要 Google Maps 付費 API）
- 沒有航班動態自動偵測 delay（需要付費航班資料服務）
- 沒有帳號系統（這趟旅行 4 人共用一份資料，夠用；若要做成給別人用的通用工具才需要）
