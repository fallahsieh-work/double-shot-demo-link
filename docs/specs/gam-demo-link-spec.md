# Spec：GAM 廣告 Demo Link 頁面

> 對應 PRD：[docs/prd/gam-demo-link.md](../prd/gam-demo-link.md)

## 版本紀錄

| 版本 | 日期 | 變更內容 |
|------|------|----------|
| v0.1 | 2026-02-02 | 初版建立 |
| v0.2 | 2026-02-02 | 灰色佔位區塊改為單一長區塊（2000px） |
| v0.3 | 2026-02-02 | 移除 ad-container 包裝，廣告 div 直接置於 body |

## 1. 檔案結構

```
project/
├── index.html          ← 主頁面（唯一需要開發的檔案）
├── claude.md
└── docs/
    ├── prd/
    │   └── gam-demo-link.md
    └── specs/
        └── gam-demo-link-spec.md
```

## 2. index.html 規格

### 2.1 HTML Head

- `<meta charset="UTF-8">`
- `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">`
- GPT JS SDK：`<script async src="https://securepubads.g.doubleclick.net/tag/js/gpt.js" crossorigin="anonymous"></script>`
- GPT Slot 定義：

```js
window.googletag = window.googletag || {cmd: []};
googletag.cmd.push(function() {
  googletag.defineSlot(
    '/31610311/supertaste_m_read_uptop_test',
    [[336, 280], 'fluid', [1, 1]],
    'div-gpt-ad-1770014037084-0'
  ).addService(googletag.pubads());
  googletag.pubads().enableSingleRequest();
  googletag.enableServices();
});
```

### 2.2 HTML Body

#### 廣告版位（無外層包裝，直接置於 body）

```html
<div id="div-gpt-ad-1770014037084-0">
  <script>
    googletag.cmd.push(function() {
      googletag.display('div-gpt-ad-1770014037084-0');
    });
  </script>
</div>
```

- 不設 `min-width` / `min-height` 限制（移除 GAM 預設的 336x280 限制）
- 寬度 100%，高度 auto，讓 creative 自由展開

#### 灰色佔位區塊

- 數量：1 個連續區塊
- 高度：2000px
- 寬度：100%
- 背景色：`#E0E0E0`

### 2.3 CSS 規格

```
body {
  margin: 0;
  padding: 0;
  background: #F5F5F5;
  -webkit-overflow-scrolling: touch;   /* iOS 滑動順暢 */
}

灰色佔位區塊 {
  width: 100%;
  height: 2000px;
  background: #E0E0E0;
}
```

### 2.4 不需要實作的項目

- 無 JavaScript 互動邏輯（由 creative 控制）
- 無後端 API
- 無用戶登入/認證
- 無 analytics（Demo 用途）

## 3. 部署規格

- 平台：GitHub Pages
- 來源：main branch 根目錄的 `index.html`
- URL 格式：`https://fallahsieh-work.github.io/double-shot-demo-link/`

## 4. 開發中驗證項目

部署後需實機測試確認：

| # | 驗證項目 | 預期結果 | 失敗時處理 |
|---|---------|---------|-----------|
| 1 | 素材 1 是否正確顯示 | 寬度 100%，高度隨素材比例 | 調整廣告容器 CSS |
| 2 | 滑動後素材 2 sticky banner 是否出現 | 螢幕頂端顯示 320x100 | 確認 creative 能否取得 scroll 事件 |
| 3 | 素材 3 GIF 掉落動畫 | 從頂部掉落至底部 | 確認 SafeFrame 設定 |
| 4 | 素材 4 底部滑出 | 從螢幕底部向上滑出 | 確認 creative DOM 操作權限 |
| 5 | Creative 是否需要特定 DOM 結構 | 不需要 | 依 creative 需求調整 HTML |
| 6 | Creative 是否需要額外 JS SDK | 不需要 | 依需求補入 |
