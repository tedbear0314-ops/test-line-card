# 待辦事項

這份檔案用來記錄後續工作，方便換電腦或隔一段時間後接續開發。

## 目前狀態

- 本機已完成主要修改，但不一定都已同步到 GitHub。
- 目前本機主要檔案：
  - `index.html`
  - `admin.html`
  - `image-links.json`
  - `README.md`
  - `TODO.md`
- 網頁目前分成 3 個分頁：
  - 節日定聯
  - 資訊分享
  - 手機相片
- 資訊分享包含：
  - 新聞
  - 保險
  - 財經
- GitHub 固定圖片來源：
  - `images/`
- 手機臨時照片來源：
  - Cloudinary
- 測試用後台入口：
  - `admin.html`

## 已完成

- [x] 從 GitHub `images` 資料夾讀取固定圖片。
- [x] 使用檔名自動分類圖片。
- [x] 合併新聞、保險、財經到「資訊分享」。
- [x] 保留「節日定聯」獨立分頁。
- [x] 手機相片可上傳到 Cloudinary。
- [x] 新增 `image-links.json` 作為圖片連結對照表。
- [x] 圖片分享到 LINE 時不顯示檔名。
- [x] 圖片分享到 LINE 時不點開大圖。
- [x] Flex Message 圖片目前使用 `fit`。
- [x] 選圖頁改成上下滑動。
- [x] 電腦 / 平板兩張一排。
- [x] 手機一張一排。
- [x] 新增 `README.md` 說明專案。

## 尚未同步到 GitHub 時要做

如果換電腦或準備正式上線，請確認 GitHub repo 有這些檔案：

- [ ] `index.html`
- [ ] `image-links.json`
- [ ] `README.md`
- [ ] `TODO.md`

GitHub repo：

```text
https://github.com/tedbear0314-ops/line-card
```

手動更新方式：

1. 到 GitHub repo。
2. 編輯或上傳上述檔案。
3. Commit changes。
4. 等 GitHub Pages 更新。

## 圖片與命名規則

固定圖片放在 GitHub 的 `images/` 資料夾。

分類規則：

```text
檔名包含 保險 / 財經 / 新聞
或 insurance / finance / news
→ 資訊分享

其他圖片
→ 節日定聯
```

待補內容：

- [ ] 節日定聯圖片 5 到 10 張。
- [ ] 新聞圖片 2 到 5 張。
- [ ] 保險資訊圖片 2 到 5 張。
- [ ] 財經資訊圖片 2 到 5 張。
- [ ] 測試至少 2 張有連結的圖片。

## `image-links.json` 待辦

目前 `image-links.json` 用來設定圖片與連結。

格式：

```json
{
  "新聞_春捲中毒.png": "https://example.com/news",
  "保險_長照險.png": "https://example.com/product",
  "財經_退休規劃.png": "https://example.com/article"
}
```

待辦：

- [ ] 補上測試圖片連結。
- [ ] 測試 LINE 裡點圖片是否能開網址。
- [ ] 刪除圖片時，同步刪除對應連結。

注意：

- 圖片檔名要完全一致。
- 除了最後一行，每一行後面要有逗號。

## Cloudinary 待辦

目前設定：

```js
const CLOUDINARY_CLOUD_NAME = "dlknzcex3";
const CLOUDINARY_UPLOAD_PRESET = "line_card_phone";
const CLOUDINARY_UPLOAD_FOLDER = "line_card_phone";
```

待辦：

- [ ] 用手機測試上傳照片。
- [ ] 確認 Cloudinary Media Library 裡會出現照片。
- [ ] 確認上傳後可分享到 LINE。
- [ ] 觀察 Cloudinary 使用量。
- [ ] 定期清理不需要的手機臨時照片。

## 版面測試

請分別測試：

- [ ] 電腦版。
- [ ] 平板版。
- [ ] 手機版。
- [ ] LINE 內建瀏覽器。
- [ ] 分享出去後的 LINE Flex Message。

檢查項目：

- [ ] 節日定聯是否顯示正確。
- [ ] 資訊分享是否顯示正確。
- [ ] 手機相片是否可上傳。
- [ ] 選圖後按鈕是否啟用。
- [ ] 分享到 LINE 是否成功。
- [ ] 圖片是否完整顯示。
- [ ] 有連結的圖片是否可點。
- [ ] 沒有連結的圖片是否不會動。

## 後續簡化內容管理

目前討論過兩個方向。

### 方案 A：Google 表單 + Google 試算表

目標：

```text
使用者只要填表單：
分類、圖片、連結、備註
```

待辦：

- [ ] 建立 Google 表單。
- [ ] 建立 Google 試算表。
- [ ] 設計欄位：
  - enabled
  - category
  - image_url
  - link_url
  - note
- [ ] 研究如何讓網站讀 Google Sheet。
- [ ] 評估圖片是否仍放 GitHub / Cloudinary。

### 方案 B：Cloudinary + Google Sheet 小後台

目標：

```text
使用者開一個簡單頁面：
選分類、選圖片、貼連結、按上傳
```

系統自動：

- 上傳圖片到 Cloudinary。
- 把圖片網址與連結寫入 Google Sheet。
- 網站讀 Google Sheet 顯示內容。

待辦：

- [x] 規劃小後台畫面。
- [x] 新增 `admin.html` 測試入口。
- [x] `index.html` 優先讀取 Google Sheet 測試資料。
- [ ] 研究 Google Apps Script 寫入 Sheet。
- [ ] 補上 Google Apps Script `doGet`，讓前台可讀 Sheet。
- [ ] 研究 Cloudinary 上傳與安全設定。
- [ ] 設計新增 / 編輯 / 刪除流程。

## 未來可能功能

- [ ] 數位名片 Flex 卡片。
- [ ] 分享時自動附上數位名片。
- [ ] 電話按鈕。
- [ ] LINE 好友按鈕。
- [ ] 分享按鈕。
- [ ] footer 按鈕「詳情請看」。
- [ ] 後台排序功能。
- [ ] 圖片啟用 / 停用功能。
- [ ] Cloudinary 自動刪除舊照片。
- [ ] GitHub Actions 自動部署。

## 換電腦時提醒

換電腦後請先確認：

- [ ] 是否有最新的 `index.html`。
- [ ] 是否有 `image-links.json`。
- [ ] 是否有 `README.md`。
- [ ] 是否有 `TODO.md`。
- [ ] GitHub 上的檔案是否與本機一致。
- [ ] Cloudinary 帳號是否可登入。
- [ ] Google / GitHub / Cloudinary 權限是否可用。

如果未來要用 git：

```bash
git clone https://github.com/tedbear0314-ops/line-card.git
cd line-card
```

修改後：

```bash
git add index.html image-links.json README.md TODO.md
git commit -m "update line card"
git push origin main
```

目前若沒有 GitHub 認證，就先用 GitHub 網頁手動編輯與上傳。
