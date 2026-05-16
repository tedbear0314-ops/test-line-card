# 待辦事項

本專案目前先封存，等待朋友實際試用後再更新。

## 封存狀態

- [x] 前台 `index.html` 讀取 Google Sheet。
- [x] 後台 `admin.html` 可上傳圖片到 Cloudinary。
- [x] 後台可把資料寫入 Google Sheet。
- [x] 前台不再讀 GitHub `images/`。
- [x] 前台不再讀 `image-links.json`。
- [x] 前台分成「節日定聯」、「資訊分享」、「上傳後台」。
- [x] 前台新增「開場文字」開關。
- [x] 開場文字打開後會顯示文字框，可先編輯再分享。
- [x] 開場文字會依圖片分類帶入建議文案。
- [x] 前台新增「附名片」開關。
- [x] 後台新增「數位名片」分類。
- [x] 後台新增數位名片資料欄位：名稱、公司、職稱、電話、LINE 加好友連結、一句話。
- [x] 前台會讀取 `business_card` 分類的最新啟用圖片作為附加名片。
- [x] 前台會把數位名片資料產生 Flex 名片卡，含打電話與加 LINE 按鈕。
- [x] 尚未上傳數位名片時，「附名片」開關會停用。
- [x] 數位名片 Flex 尾卡已調整成較接近節日定聯、資訊分享輪播卡的尺寸。
- [x] 數位名片照片改成 `fit`，避免裁切原圖。
- [x] 數位名片公司名稱不再重複職稱。
- [x] 後台顯示目前啟用中的圖片。
- [x] 後台有「停用」按鈕，設計上會把 `enabled` 改為 `FALSE`。
- [x] 後台已移除「最近上傳」區塊。
- [x] 後台已隱藏長網址與工程用 JSON 資訊。

## 主要檔案

```text
index.html        分享頁
share.html        收到的人轉分享用頁面
admin.html        上傳與停用管理後台
AppsScript.gs     Apps Script 範例程式碼
README.md         封存說明
TODO.md           封存待辦
image-links.json  舊版備份，目前測試版不讀取
```

## 外部服務

每套版本需要：

- GitHub Pages：放 `index.html` 與 `admin.html`。
- LINE LIFF：開啟分享頁。
- Google Sheet：存放圖片資料。
- Apps Script Web App：讀寫 Google Sheet。
- Cloudinary：存放圖片。

## 複製給朋友時必改

`index.html`：

- [ ] `LIFF_ID`
- [ ] `GOOGLE_SHEET_DATA_URL`

`admin.html`：

- [ ] `CLOUDINARY_CLOUD_NAME`
- [ ] `CLOUDINARY_UPLOAD_PRESET`
- [ ] `CLOUDINARY_UPLOAD_FOLDER`
- [ ] `GOOGLE_APPS_SCRIPT_URL`

通常可以固定：

```js
const CLOUDINARY_UPLOAD_PRESET = "line_card_phone";
const CLOUDINARY_UPLOAD_FOLDER = "line_card_phone";
```

## Apps Script 必要功能

- [ ] `doPost`：新增資料。
- [ ] `doGet`：讀取資料。
- [ ] `action: "disable"`：把指定圖片的 `enabled` 改成 `FALSE`。
- [ ] `action: "create_share_set"`：建立二次分享包。
- [ ] `action=get_share_set&id=...`：讀取二次分享包。

每次改 Apps Script 後都要：

```text
部署 → 管理部署作業 → 編輯 → 版本選新增版本 → 部署
```

## 朋友試用後觀察

- [ ] 是否知道要先進「上傳後台」。
- [ ] 是否能正確選擇「節日定聯 / 資訊分享」。
- [ ] 是否會貼錯連結或漏掉 `https://`。
- [ ] 圖片上傳速度是否可以接受。
- [ ] 上傳後回分享頁是否直覺。
- [ ] 停用圖片是否好理解。
- [ ] 分享到 LINE 是否成功。
- [ ] 開場文字開關是否好理解。
- [ ] 開場文字預設文案是否自然。
- [ ] 附名片開關是否好理解。
- [ ] 名片圖片在 LINE 裡是否完整顯示。
- [ ] LINE Flex Message 圖片是否完整顯示。
- [ ] 有連結的圖片是否可點。
- [ ] 沒有連結的圖片是否不會誤觸。

## 未來可能功能

- [x] 新增 `share.html`，讓收到的人可以轉分享指定的一組內容。
- [x] 分享時建立分享包，並在名片尾卡加入「轉分享」按鈕。
- [ ] 實機測試 `share.html?id=...` 是否能在 LINE 內正常開啟並轉分享。
- [ ] 評估是否讓沒有附名片的分享也能放入「轉分享」入口。
- [ ] 評估是否新增獨立名片分享頁，讓名片本身可以單獨打開、單獨分享。
- [ ] 評估資訊分享是否新增文章型 Flex 卡片：圖片、標題、摘要、詳情按鈕。
- [ ] 後台新增「編輯」功能。
- [ ] 後台新增排序功能。
- [ ] 後台新增啟用 / 停用切換。
- [ ] 後台新增簡單密碼或保護機制。
- [ ] 圖片 footer 加「詳情請看」按鈕。
- [ ] Cloudinary 舊圖清理流程。
- [ ] 建立朋友版複製流程文件。

## 注意事項

- 前端不能放 Cloudinary API Secret。
- 刪 Cloudinary 圖片不等於從分享頁移除，分享頁是否顯示由 Google Sheet 的 `enabled` 決定。
- 若 Cloudinary 圖片被刪除，舊 LINE 訊息可能破圖。
- `image-links.json` 目前只作為舊版備份，不影響測試版功能。
