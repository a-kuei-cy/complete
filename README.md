# 興嘉國小雲端作品平臺 — 登入通訊修正版

## 1. Apps Script
1. 將 `Code.gs` 全部貼入原 Google Apps Script 專案，儲存。
2. 在「專案設定 → 指令碼屬性」確認：
   - `SHEET_ID`：既有試算表 ID
   - `FOLDER_ID`：既有 Drive 資料夾 ID
   - `SITE_ORIGIN`：`https://a-kuei-cy.github.io`（不可包含 `/complete/`）
   - 原有 `ADMIN_PASSWORD_HASH` 和 `ADMIN_PASSWORD_SALT` 保留。
3. 在編輯器執行 `testConnection`，查看執行記錄。
4. 「部署 → 管理部署作業 → 編輯 → 新版本 → 部署」。
5. 執行身分選「我」、存取權限選「任何人」。

## 2. GitHub Pages
將 `index.html`、`config.js`、`sceslogo.png` 上傳到 `complete` 儲存庫根目錄，等待 Pages 更新。
`Code.gs` 不要上傳 GitHub。

## 3. 管理者密碼安全重設（先前密碼已公開，務必更換）
1. 在 Apps Script「專案設定 → 指令碼屬性」暫時新增 `ADMIN_PASSWORD_RESET_ONCE`，值填**全新的強密碼**（至少 12 字元，不要在聊天中提供）。
2. 在編輯器選擇 `setupAdminPassword` 並執行。
3. 程式會重新產生 `ADMIN_PASSWORD_HASH`、`ADMIN_PASSWORD_SALT`，並自動刪除 `ADMIN_PASSWORD_RESET_ONCE`。
4. 再到指令碼屬性確認一次性屬性已不存在。注意：這會更新 Script Properties，不必因此重新部署程式。

## 4. 測試
- 在瀏覽器開啟 Apps Script `/exec?action=ping`，預期回傳 `{"ok":true,"message":"後端已連線"}`。
- `/exec?action=list` 預期回傳 `{"ok":true,"works":[...]}`，若回傳錯誤請依訊息檢查試算表權限。
- 在網站點「密碼登入」，管理者帳號欄位留空，輸入新密碼。
- 登入成功後測試新增上傳者帳號、上傳作品、刪除自己作品及管理者刪除功能。

## 5. 注意
此版修正了 iframe 回傳目標及回應來源判斷，尚未在實際 Google 部署環境進行端到端測試。
如果仍逾時，請提供 Apps Script「執行項目」中 `doPost` 的執行結果及錯誤訊息（遮住任何密碼、權杖）。
