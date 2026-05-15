# LINE LIFF 圖片分享卡片

這個專案是一個 LINE LIFF 圖片分享工具。使用者在網頁中選取圖片後，可以分享到 LINE，並以 Flex Message 圖片輪播顯示。

目前用途是整理並分享：

- 節日定聯圖片
- 新聞、保險、財經等資訊圖片
- 手機臨時照片

## 目前功能

- 從 GitHub repo 的 `images` 資料夾自動讀取圖片。
- 依照檔名自動分類圖片。
- 選取多張圖片後分享到 LINE。
- LINE 裡用 Flex Message carousel 顯示。
- 圖片不顯示檔名。
- 圖片不會點開大圖。
- 圖片使用 `fit` 顯示，盡量保留完整比例。
- 手機臨時照片可上傳到 Cloudinary，當次選取並分享。
- 可用 `image-links.json` 讓指定圖片連到文章或商品頁。

## 檔案結構

```text
line-card/
  index.html          主程式與頁面
  admin.html          測試用上傳後台
  image-links.json    圖片檔名與外部連結的對照表
  images/             固定圖片資料夾
```

## 測試用上傳後台

`admin.html` 是新測試入口，目標是讓沒有電腦背景的使用者可以用簡單表單新增圖片。

目前後台流程：

```text
選分類：節日定聯 / 資訊分享
選圖片
貼上新聞或保險 DM 連結
送出
```

目前已可把圖片上傳到 Cloudinary。若要自動寫入 Google Sheet，需先建立 Google Apps Script Web App，並把網址填到 `admin.html` 裡的 `GOOGLE_APPS_SCRIPT_URL`。

正式使用前，建議先把 `admin.html` 當測試頁，不影響原本的 `index.html` 分享頁。

## 分頁與分類

目前網頁有 3 個分頁：

```text
節日定聯 | 資訊分享 | 手機相片
```

分類規則：

```text
檔名包含 保險 / 財經 / 新聞
或 insurance / finance / news
→ 資訊分享

其他圖片
→ 節日定聯
```

範例：

```text
新聞_春捲中毒.png      → 資訊分享
財經_退休規劃.png      → 資訊分享
保險_長照險.png        → 資訊分享
母親節祝福.png         → 節日定聯
中秋節問候.png         → 節日定聯
```

## 圖片連結設定

如果某張圖片要連到文章、新聞、商品頁，請編輯 `image-links.json`。

格式如下：

```json
{
  "新聞_春捲中毒.png": "https://example.com/news",
  "保險_長照險.png": "https://example.com/product",
  "財經_退休規劃.png": "https://example.com/article"
}
```

注意：

- 圖片檔名必須和 `images` 資料夾裡的檔名完全一樣。
- 除了最後一行，每一行後面都要有逗號。
- 沒有出現在 `image-links.json` 的圖片，分享到 LINE 後不能點連結。
- 目前連結是加在 Flex Message 圖片上，不是 footer 按鈕。

## Google Sheet 測試資料來源

測試版前台 `index.html` 目前會優先讀 Google Sheet 資料。讀不到 Google Sheet 時，才會退回原本的 GitHub `images/` 與 `image-links.json`。

Google Sheet 欄位：

```text
enabled | category | image_url | link_url | note | created_at
```

分類值：

```text
program → 節日定聯
info    → 資訊分享
```

前台讀取網址設定在 `index.html`：

```js
const GOOGLE_SHEET_DATA_URL = "Google Apps Script Web App 網址";
```

Apps Script 需要同時支援：

- `doPost`：讓 `admin.html` 寫入資料。
- `doGet`：讓 `index.html` 讀取資料。

## 手機相片與 Cloudinary

手機相片是臨時上傳分享用。

目前 Cloudinary 設定在 `index.html`：

```js
const CLOUDINARY_CLOUD_NAME = "dlknzcex3";
const CLOUDINARY_UPLOAD_PRESET = "line_card_phone";
const CLOUDINARY_UPLOAD_FOLDER = "line_card_phone";
```

目前行為：

- 使用者在「手機相片」分頁選照片。
- 照片會上傳到 Cloudinary。
- 上傳成功後，當次頁面可以選取並分享到 LINE。
- 重新整理頁面後，不會自動列出之前上傳的手機照片。
- Cloudinary 後台仍然可以看到已上傳的照片。

重要提醒：

- LINE 分享出去的是圖片網址，不是把圖片複製進 LINE。
- 如果 GitHub 或 Cloudinary 上的圖片被刪除，對方 LINE 裡的舊訊息之後可能會破圖。

## 圖片尺寸建議

建議尺寸：

```text
節日圖片：1200 x 800 px
資訊圖片：1040 x 1560 px
```

資訊圖片包含新聞、保險、財經等有文字的卡片。

建議：

- 圖片四周留 60 到 120 px 安全邊界。
- 不要做太長，例如 1080 x 3000，LINE 裡會顯得太小。
- 檔案大小盡量控制在 1MB 到 3MB。
- 照片多用 JPG，文字圖可用 PNG。

## 版面規則

選圖頁目前使用直向排列：

```text
電腦 / 平板：兩張一排，上下滑動
手機：一張一排，上下滑動
```

LINE 分享出去後仍是 Flex Message carousel，會左右滑動。這是 LINE carousel 的正常行為。

## 上線流程

目前本機資料夾不一定是 Git repo，因此更新 GitHub 時可先用手動方式：

1. 到 GitHub repo。
2. 更新 `index.html`。
3. 更新或新增 `image-links.json`。
4. 上傳或刪除 `images` 資料夾中的圖片。
5. Commit changes。

未來若設定好 GitHub 認證，也可以改用：

```bash
git add index.html image-links.json
git commit -m "update line card"
git push origin main
```

## 刪除圖片

在 GitHub 網頁刪除圖片：

1. 進入 repo 的 `images` 資料夾。
2. 點要刪的圖片。
3. 使用右上角的刪除功能。
4. Commit changes。
5. 如果 `image-links.json` 裡有該圖片，也刪除對應那一行。

注意：已經分享給客戶的重要圖片不要太快刪，否則舊 LINE 訊息可能會破圖。

## 開發日誌

### 2026-05-14

- 建立主要 LIFF 圖片分享頁。
- 從 GitHub `images` 資料夾讀取圖片。
- 支援選圖後分享到 LINE。
- LINE 中用 Flex Message carousel 顯示圖片。
- 不顯示檔名。
- 圖片不點開大圖。
- 圖片使用 `fit` 保留完整比例。
- 接上 Cloudinary 手機相片上傳。
- Cloudinary 使用 unsigned preset：`line_card_phone`。

### 2026-05-15

- 討論新聞、保險、財經圖片的使用方式。
- 新增 `image-links.json`，讓指定圖片可連到文章或商品頁。
- 曾新增多個分頁：保險資訊、財經資訊、新聞分享。
- 後續為了簡化介面，合併為 3 個分頁：
  - 節日定聯
  - 資訊分享
  - 手機相片
- 將新聞、保險、財經類圖片統一歸入「資訊分享」。
- 將選圖頁改成直向排列：
  - 電腦 / 平板兩張一排
  - 手機一張一排
- 曾測試新聞圖片用 `cover`，後來改回全部使用 `fit`。

## 未來可能功能

- 小後台：讓使用者用表單新增圖片、分類、連結。
- Google Sheet 內容管理：用試算表取代手動編輯 JSON。
- 「詳情請看」footer 按鈕：讓圖片不能點，只有底部按鈕可點。
- Cloudinary 自動清理舊照片。
- GitHub Actions 自動部署或維護資料。
