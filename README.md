# 興嘉國小雲端作品分享平臺（不使用 Google OAuth）

## 檔案
- `index.html`：網站首頁、搜尋分類、分頁、分享 QR Code、管理者登入及上傳。
- `config.js`：填入 Apps Script 的 `/exec` 部署網址。
- `Code.gs`：Google Apps Script 後端。
- `sceslogo.png`：興嘉國小校徽。

## 安裝
1. 建立 Google 試算表，複製網址中 `/d/` 後的試算表 ID。
2. 開啟 Apps Script 專案，將 `Code.gs` 貼入。
3. 專案設定 → 指令碼屬性：
   - `SHEET_ID`：試算表 ID（必填）。
   - `FOLDER_ID`：已預設為 `1F94FETdzguMxWyZheYxvuCie5X-OmDi8`，可省略。
   - `SITE_ORIGIN`：GitHub Pages 的來源，例如 `https://yourname.github.io`，**不含路徑或尾端斜線**。
4. 在 `Code.gs` 的 `setupAdminPassword()` 將範例密碼替換成**自己設定的至少 12 字元強密碼**，在 Apps Script 編輯器執行一次，完成授權後**立即刪掉程式中的明文密碼**。正式使用時不要保留範例密碼。
5. Apps Script → 部署 → 新增部署 → 網頁應用程式：執行身分「我」、誰可以存取「任何人」，完成授權並取得 `/exec` 網址。
6. 編輯 `config.js` 的 `GAS_URL`，填入 `/exec` 網址。
7. GitHub 新增儲存庫，將 `index.html`、`config.js`、`sceslogo.png` 放在根目錄，Settings → Pages → Deploy from a branch → main / root。
8. 開啟 GitHub Pages 網址，測試作品列表、管理登入、上傳、刪除及 QR Code 分享。

## 重要提醒
- **完全不需要 Google Cloud OAuth 網頁用戶端 ID。**
- Apps Script 以 Script Properties 儲存管理密碼的加鹽雜湊，不在 GitHub 保存密碼。
- 登入採短效管理憑證（約 6 小時），儲存在瀏覽器 sessionStorage。
- 登入錯誤次數限制是 Apps Script Cache 的簡易全站限制，不是針對 IP 的完整防暴力破解防護。請使用高強度密碼；學校正式對外服務宜再加強防護。
- 上傳檔案預設上限 5MB，Google Apps Script 的表單/執行配額仍可能影響較大檔案。
- Google Workspace 管理員可能限制 Drive 公開分享；此時公開作品連結可能無法存取。
- Google Drive 的 HTML 檔不保證能直接以互動網站方式執行，互動 HTML 建議另行部署 GitHub Pages。
- QR Code 使用外部圖片服務，作品分享網址會傳給該服務。
- 作品會公開，請確認學生個資、照片肖像及著作權授權。
- 跨網站 iframe 與 Apps Script 的 postMessage 流程需在實際 GitHub Pages 網址及目標瀏覽器上測試。
- 目前已產生程式碼並檢查主要檔案與欄位，但**尚未連接實際 Google 試算表與 Apps Script 執行線上端對端測試**。

## 本次已設定
`config.js` 已填入提供的 Apps Script /exec 網址。請另外確認 Apps Script 指令碼屬性 `SHEET_ID`、`SITE_ORIGIN`，並將 `index.html`、`config.js`、`sceslogo.png` 上傳 GitHub Pages。若修改 `Code.gs`，請在 Apps Script 重新部署新版本。


## 新版：雙角色密碼登入
- 管理者：登入時「上傳者帳號」留空，輸入原管理密碼。可上傳、刪除全部作品、新增/重設/停用上傳者帳號。
- 上傳者：使用管理者建立的帳號與密碼登入；可上傳及刪除自己上傳的作品，不可管理其他帳號或刪除他人作品。
- 舊作品沒有 ownerId 欄位資料，只有管理者可以刪除。
- `作品資料` 工作表會自動補第八欄 `ownerId`。不要手動改動欄位順序。
- 上傳者帳號及密碼雜湊儲存在 Apps Script 指令碼屬性 `UPLOADER_ACCOUNTS`。勿公開或手動修改。
- 更換 `Code.gs` 後，請到 Apps Script「部署 > 管理部署作業 > 編輯 > 版本：新版本 > 部署」，確保 `/exec` 使用新版。
- GitHub Pages 更新 `index.html`、`config.js`、`sceslogo.png`；不要將 `Code.gs` 或任何密碼放在 GitHub。
- `SITE_ORIGIN` 請設為 `https://a-kuei-cy.github.io`。
- 密碼登入依賴 Apps Script 跨網域 iframe 回傳；請在正式網站測試登入、上傳、刪除及帳號停用。
