# LINE LIFF 圖片分享卡片測試版

本資料夾目前作為新版流程測試版封存。原本穩定版是使用 GitHub `images/` 與 `image-links.json` 管理圖片與連結；本測試版已改成：

```text
admin.html 上傳圖片
→ Cloudinary 存放圖片
→ Google Sheet 記錄資料
→ index.html 讀取 Google Sheet
→ LINE LIFF 分享圖片
```

這版先暫停在可供朋友試用的狀態。後續等朋友實際使用後，再依照回饋調整。

## 目前狀態

- `index.html`：分享頁。
- `admin.html`：上傳與停用管理後台。
- `image-links.json`：舊版 GitHub 圖片連結對照表，目前測試版前台不讀取，可先保留備份。
- 前台只讀 Google Sheet，不再讀 GitHub `images/`。
- 圖片本體存放在 Cloudinary。
- 分享到 LINE 時使用 Flex Message carousel。
- 分享頁可選擇是否加入開場文字；開啟後會出現文字框，可先編輯再分享。
- 分享頁可選擇是否附上數位名片；名片由上傳後台新增，不寫死在 GitHub。
- 圖片顯示使用 `fit`，盡量保留完整比例。
- 目前主要分享分類：
  - `program`：節日定聯
  - `info`：資訊分享
- 另有 `business_card`：數位名片，作為分享時的附加尾卡，不會出現在節日定聯或資訊分享列表。

## 頁面分工

### `index.html`

給使用者選圖片並分享到 LINE。

前台按鈕：

```text
節日定聯 | 資訊分享 | 上傳後台
```

其中「上傳後台」會直接開啟 `admin.html`。

分享工具列：

```text
已選張數 | 開場文字開關 | 附名片開關 | 分享到 LINE
```

「開場文字」預設關閉。打開後會出現文字框，系統會依目前選取的圖片分類帶入建議文字，使用者可自行修改。分享時若文字框有內容，會先送出文字訊息，再送出圖片輪播；若文字框空白，則只送圖片輪播。

「附名片」預設關閉。若後台已有上傳啟用中的數位名片，打開後會把名片加在圖片輪播最後，作為分享署名卡。若尚未上傳數位名片，前台開關會停用，不能打開。

LINE 一次最多可送 5 則訊息物件。現在的限制是：

- 不加開場文字：最多 60 張圖片。
- 加開場文字：最多 48 張圖片。
- 若同時附名片，因名片也算 1 張，最多可選圖片數會再少 1 張。

### `admin.html`

給朋友或管理者新增圖片。

後台流程：

```text
選分類
選圖片
貼新聞或保險 DM 連結
填備註
送出
```

後台會：

- 上傳圖片到 Cloudinary。
- 寫入 Google Sheet。
- 顯示目前啟用中的圖片。
- 可按「停用」把 Google Sheet 的 `enabled` 改成 `FALSE`。
- 分類選「數位名片」時，會顯示名片資料欄位，供前台產生 Flex 名片卡。

停用只會讓分享頁不顯示該圖片，不會刪除 Cloudinary 圖片檔。

## Google Sheet

欄位固定如下：

```text
enabled | category | image_url | link_url | note | display_name | company_name | job_title | phone | line_friend_url | tagline | created_at
```

欄位用途：

- `enabled`：是否顯示，`TRUE` 顯示，`FALSE` 不顯示。
- `category`：`program`、`info` 或 `business_card`。
- `image_url`：Cloudinary 圖片網址。
- `link_url`：新聞、文章、保險 DM 等外部連結，可空白。
- `note`：管理用備註。
- `display_name`：數位名片顯示名稱。
- `company_name`：公司或單位。
- `job_title`：職稱。
- `phone`：電話，數位名片按鈕會用。
- `line_friend_url`：LINE 加好友連結，數位名片按鈕會用。
- `tagline`：數位名片一句話。
- `created_at`：建立時間。

數位名片使用同一張表儲存，`category` 填 `business_card`。前台會讀取啟用中的數位名片，並使用最新一筆產生 Flex 名片卡，放在圖片輪播最後。

若 Apps Script 是依照 Google Sheet 欄位標題寫入資料，新增欄位後要重新部署 Apps Script 新版本。

## Apps Script

Apps Script Web App 需要支援：

- `doPost`：讓 `admin.html` 新增資料。
- `doGet`：讓 `index.html` 和 `admin.html` 讀取資料。
- `action: "disable"`：讓 `admin.html` 停用圖片。

部署設定：

```text
執行身分：我
誰可以存取：任何人
```

每次修改 Apps Script 後，必須重新部署新版本。

## Cloudinary

目前測試版使用 unsigned upload preset。

`admin.html` 內需要設定：

```js
const CLOUDINARY_CLOUD_NAME = "Cloudinary cloud name";
const CLOUDINARY_UPLOAD_PRESET = "line_card_phone";
const CLOUDINARY_UPLOAD_FOLDER = "line_card_phone";
```

若幫朋友各自開 Cloudinary 帳號，建議都建立同名 preset：

```text
line_card_phone
```

這樣每套朋友版通常只需要換 `CLOUDINARY_CLOUD_NAME`。

不要把 Cloudinary API Secret 放進前端網頁。

## LINE LIFF

`index.html` 內需要設定：

```js
const LIFF_ID = "LIFF ID";
```

目前測試版已改過 LIFF ID。若複製給朋友使用，需要替換成朋友自己的 LIFF ID，並在 LINE Developers 將 Endpoint URL 指到該朋友的 GitHub Pages 網址。

朋友若使用自己的 LINE Developers Channel，認證畫面會顯示朋友自己的服務名稱與提供者。

## 幫朋友複製一套時要改的地方

`index.html`：

```js
const LIFF_ID = "朋友的 LIFF ID";
const GOOGLE_SHEET_DATA_URL = "朋友的 Apps Script Web App URL";
```

`admin.html`：

```js
const CLOUDINARY_CLOUD_NAME = "朋友的 Cloudinary cloud name";
const CLOUDINARY_UPLOAD_PRESET = "line_card_phone";
const CLOUDINARY_UPLOAD_FOLDER = "line_card_phone";
const GOOGLE_APPS_SCRIPT_URL = "朋友的 Apps Script Web App URL";
```

GitHub repo 名稱不同時，通常不需要改 `index.html` 或 `admin.html` 的路徑，因為目前使用的是相對路徑：

```text
admin.html
index.html
```

但 LINE Developers 的 LIFF Endpoint URL 要改成新 repo 的 GitHub Pages 網址。

## 圖片尺寸建議

```text
節日圖片：1200 x 800 px
資訊圖片：1040 x 1560 px
```

建議：

- 圖片四周留 60 到 120 px 安全邊界。
- 資訊圖不要做太長，例如 1080 x 3000，LINE 裡會顯得太小。
- 檔案大小盡量控制在 1MB 到 3MB。
- 照片多用 JPG，文字圖可用 PNG。

## 暫停點

本測試版目前先封存，等朋友試用後再依需求更新。

後續優先觀察：

- 朋友是否能順利上傳圖片。
- 分類是否容易理解。
- 貼連結是否會漏掉 `https://`。
- 停用圖片是否好理解。
- LINE 分享出去後圖片是否完整顯示。
- 有連結的圖片點擊是否正常。
