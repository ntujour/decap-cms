# Decap CMS — 台大新聞所內容管理系統

## 專案概述

為國立臺灣大學新聞研究所建立的 Decap CMS 內容管理系統，管理四類內容：師資 (faculty)、學生發表 (publications)、最新消息 (news)、活動 (activities)。內容以 Markdown + YAML frontmatter 格式儲存，圖片存於 repo 內，部署至 GitHub Pages。

## 開發指令

```bash
# 本地測試（使用 Decap CMS proxy server）
npx decap-server

# 或使用任意靜態伺服器
python -m http.server 8080
# 然後開啟 http://localhost:8080/admin/
```

## 主要檔案索引

| 檔案 | 說明 |
|------|------|
| `admin/index.html` | CMS 管理介面入口 |
| `admin/config.yml` | CMS 核心設定檔（collections 定義） |
| `content/faculty/*.md` | 師資資料 |
| `content/publications/*.md` | 學生發表 |
| `content/news/*.md` | 最新消息 |
| `content/activities/*.md` | 活動資訊 |
| `images/` | 圖片資源（依 collection 分子目錄） |

## 架構說明

### Collections

| Collection | 資料夾 | Slug 格式 |
|------------|--------|-----------|
| faculty（師資） | `content/faculty` | `{{slug}}`（姓名） |
| publications（學生發表） | `content/publications` | `{{year}}-{{month}}-{{slug}}` |
| news（最新消息） | `content/news` | `{{year}}-{{month}}-{{day}}-{{slug}}` |
| activities（活動） | `content/activities` | `{{year}}-{{month}}-{{day}}-{{slug}}` |

### 圖片管理

圖片依 collection 分別存放於 `images/` 下的對應子目錄：
- `images/faculty/` — 教師大頭照
- `images/publications/` — 論文封面圖
- `images/news/` — 新聞圖片
- `images/activities/` — 活動圖片

## 注意事項

- `admin/config.yml` 中的 `backend.repo` 需替換為實際 GitHub repo 路徑
- locale 設定為 `zh_Hant`（繁體中文介面）
- 部署前需在 GitHub OAuth 設定 Decap CMS 認證

## Pending Tasks (待辦事項)

> 此區塊記錄未完成的任務，Claude 啟動時自動載入。

- [ ] 替換 config.yml 中的 GitHub repo 路徑
- [ ] 設定 GitHub OAuth 認證
- [ ] 部署至 GitHub Pages

---
*Last updated: 2026-02-10*
