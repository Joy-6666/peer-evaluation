# 拾光・校園作品投票平台

以 Vite、Firebase Authentication、Cloud Firestore 建立的響應式作品投稿與投票網站，可部署至 GitHub Pages。

## 本機啟動

1. 安裝 Node.js 24.12 或更新版本。
2. 複製 `.env.example` 為 `.env.local`，填入 Firebase Console 的 Web App 設定，以及管理員 Email（多位管理員用逗號分隔）。
3. 在 Firebase Authentication 開啟 Google 登入；在 Firestore 建立資料庫，並發布本專案的 `firestore.rules`。
4. 執行 `npm install`、`npm run dev`。

第一次以管理員登入時，平台會建立 `settings/main` 預設活動設定。請在 Firebase Authentication 的 Google 登入設定中，將正式網站網域加入授權網域。

## GitHub Pages 部署

GitHub Actions 工作流程已包含 Firebase Web App 公開設定，推送至 `main` 即會建置網站。第一次部署前，請在儲存庫 **Settings → Pages** 選擇 GitHub Actions 作為部署來源。工作流程會依儲存庫名稱自動設定 Pages 子路徑。

## Firebase 資料與安全

- `works/{id}` 存作品、壓縮圖片與作者名稱；`tallies/{id}` 單獨保存票數，盲測時 Firebase 規則會拒絕一般使用者讀取票數。
- `ballots/{uid}` 存每位 Google 使用者的投票清單；只有本人與管理員可讀。管理員可下載結果和實名投票明細 CSV。
- `settings/main` 即時控制投稿、投票、票數公開與每人票數上限。
- 請務必在 Firebase 發布 `firestore.rules`。規則中的管理員 Email 白名單要與 `VITE_ADMIN_EMAILS` 一致；多位管理員可加入同一個白名單陣列，並在 `VITE_ADMIN_EMAILS` 以逗號分隔。規則提供登入者投稿、作者編輯、票數上限及配對票數增減的權限檢查。

圖片放在 Firestore 會增加文件大小和讀取用量；此實作適合小型班級活動。大型活動建議改用 Firebase Storage。
