# one_page_template

一頁式產品行銷網頁模板。純 HTML + CSS + 原生 JavaScript，無建置流程，支援 RWD。

適用於 landing page、單品產品頁、活動頁。不含購物車、會員系統與後台，
購買行為透過外部連結（電商平台、LINE、電話）完成。

## 快速開始

直接用瀏覽器開啟 `index.html` 即可預覽。

若要以本機伺服器預覽（避免部分瀏覽器對本機檔案的限制）：

```bash
python3 -m http.server 8000
```

接著開啟 http://localhost:8000

## 檔案結構

```text
one_page_template/
├── index.html              主頁面，所有區塊都在這裡
├── assets/
│   ├── css/
│   │   ├── tokens.css      設計變數（色彩、字級、間距）
│   │   ├── base.css        reset 與全域排版
│   │   ├── layout.css      container、按鈕、網格、卡片等共用元件
│   │   └── sections.css    各區塊專屬樣式
│   ├── js/
│   │   └── main.js         漢堡選單、進場動畫、導覽高亮
│   └── images/             圖片素材放置處
├── .nojekyll               讓 GitHub Pages 跳過 Jekyll 處理
├── CONTENT.md              素材需求清單
└── README.md
```

## 頁面區塊

| 區塊 | 錨點 | 用途 |
| --- | --- | --- |
| Header | — | 固定導覽列，768px 以下切換為漢堡選單 |
| Hero | `#top` | 首屏主視覺、產品名、主副標語、關鍵賣點 |
| 產品簡介 | `#intro` | 50–100 字介紹與產品展示照 |
| 特點功效 | `#features` | 3–4 張特點卡片 |
| 成分優勢 | `#ingredients` | 成分清單、技術說明、認證標章 |
| 使用情境 | `#scenario` | 情境照圖庫 |
| 使用方法 | `#howto` | 步驟流程與建議用量 |
| 注意事項 | `#notice` | 適用範圍、警語、保存方式 |
| 常見問題 | `#faq` | 問答與退換貨說明，原生 `<details>` 摺疊 |
| 行動呼籲 | `#cta` | 購買連結、LINE、優惠說明 |
| 聯絡資訊 | `#contact` | 電話、Email、LINE、地址 |
| Footer | — | 公司全名、統編、頁尾導覽 |

每個區塊的 HTML 內都有兩種說明：開發者用的 HTML 註解，以及頁面上可見的黃色提示框
（`.note`），列出該區塊需要哪些文案與圖片。

## 客製步驟

### 1. 換品牌配色

編輯 `assets/css/tokens.css`，只需改這幾個變數：

```css
--color-brand: #2f6f4e;        /* 品牌主色 */
--color-brand-dark: #24563d;   /* 主色加深約 12%，用於 hover */
--color-brand-light: #e8f2ec;  /* 主色淡化，用於淺色底 */
--color-accent: #d98b3c;       /* 強調色，用於主要按鈕 */
```

記得同步修改 `index.html` 中的 `<meta name="theme-color">`。

### 2. 換字體

修改 `tokens.css` 的 `--font-sans`，將品牌字體加在最前方。
若改用其他 Google Fonts，同時更新 `index.html` 的字體 `<link>`。

### 3. 放入圖片

把圖片放進 `assets/images/`，再把對應的 `.media-placeholder` 區塊換成 `<img>`：

```html
<img src="assets/images/product-01.png" alt="產品名稱正面照"
     width="1400" height="1050" loading="lazy">
```

Hero 主視覺是首屏圖片，請改用 `fetchpriority="high"` 且**不要**加 `loading="lazy"`。
`width` 與 `height` 請填實際像素值，避免載入時版面跳動。

### 4. 填入文案

依 `CONTENT.md` 清單逐項替換 `index.html` 中的佔位文字。

### 5. 上線前清理

- 刪除所有 `<div class="note">...</div>` 提示框
- 刪除剩餘未替換的 `.media-placeholder`
- 更新 `<title>`、`meta description`、`og:*`、`canonical` 的網址
- 放入 `favicon.ico` 並取消 `<head>` 中該行的註解
- 補上實際的購買連結與 LINE 連結（目前為 `href="#"`）

若想先預覽「清理後」的樣子，可暫時在 `<body>` 加上 `class="is-live"`，
會一次隱藏所有提示框與圖片佔位框。

## 增減區塊

每個區塊都是獨立的 `<section class="section">`，可整段複製、搬移或刪除。
刪除區塊時記得一併移除 Header 與 Footer 導覽中對應的錨點連結。

常用的版面 class：

- `section--soft` / `section--brand` / `section--dark`：切換區塊底色
- `container--narrow`：較窄的內容寬度，適合純文字區塊
- `grid--2` / `grid--3` / `grid--4`：響應式欄位數

## 瀏覽器支援

支援最近兩個版本的 Chrome、Edge、Firefox、Safari。
使用了 `IntersectionObserver`、CSS 變數與 `aspect-ratio`；
JavaScript 停用時頁面內容仍完整可讀，FAQ 摺疊也能正常運作。

## 部署

純靜態檔案，可直接部署至 GitHub Pages、Netlify、Cloudflare Pages、Vercel
或任何靜態主機，無需建置指令。

### GitHub Pages

推上 GitHub 後，到 repo 的 **Settings → Pages**，Source 選 **Deploy from a branch**，
分支選 `main`、資料夾選 `/ (root)`，儲存後等一兩分鐘即可。

專案內已包含空的 `.nojekyll`，用來跳過 GitHub Pages 預設的 Jekyll 處理，
避免底線開頭的檔案或資料夾被忽略。

專案站點網址會多一層子目錄（`https://<username>.github.io/<repo>/`），
本模板所有資源都使用相對路徑，不需額外調整。但 `og:image` 必須改為
**完整絕對網址**，社群平台才抓得到分享縮圖：

```html
<meta property="og:image" content="https://<username>.github.io/<repo>/assets/images/og-image.jpg">
```

若要綁自訂網域，在根目錄新增 `CNAME` 檔案並填入網域名稱，
再將 DNS 的 CNAME 記錄指向 `<username>.github.io`。
