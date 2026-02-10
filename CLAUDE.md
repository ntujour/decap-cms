# Decap CMS — 台大新聞所內容管理系統

## 專案概述

為國立臺灣大學新聞研究所建立的 Decap CMS 內容管理系統，管理四類內容：師資 (faculty)、學生發表 (publications)、最新消息 (news)、活動 (activities)。內容以 Markdown + YAML frontmatter 格式儲存，圖片存於 repo 內。

- **GitHub Repo**: `ntujour/decap-cms`
- **CMS 網址**: https://monumental-cuchufli-928165.netlify.app/admin/
- **部署平台**: Netlify（hosting + GitHub OAuth proxy）
- **認證方式**: GitHub OAuth（透過 Netlify 代理）

## 開發指令

```bash
# 本地測試需同時開兩個 terminal

# Terminal 1：Decap local proxy server
npx decap-server

# Terminal 2：靜態伺服器
python3 -m http.server 8000
# 開啟 http://localhost:8000/admin/
```

> 本地測試需在 `admin/config.yml` 最上方加 `local_backend: true`，push 前記得移除。

## 主要檔案索引

| 檔案 | 說明 |
|------|------|
| `admin/index.html` | CMS 管理介面入口 |
| `admin/config.yml` | CMS 核心設定檔（collections 定義） |
| `content/faculty/*.md` | 師資資料（29 筆） |
| `content/publications/*.md` | 學生發表 |
| `content/news/*.md` | 最新消息（16 筆） |
| `content/activities/*.md` | 活動資訊（51 筆） |
| `images/` | 圖片資源（依 collection 分子目錄） |
| `data/*.json` | 網站設定檔（banner、intro、navigation、site_settings） |
| `README.md` | 部署與 OAuth 設定文件 |

## 架構說明

### Collections

| Collection | 資料夾 | Slug 格式 |
|------------|--------|-----------|
| faculty（師資） | `content/faculty` | `{{slug}}`（姓名） |
| publications（學生發表） | `content/publications` | `{{year}}-{{month}}-{{slug}}` |
| news（最新消息） | `content/news` | `{{year}}-{{month}}-{{day}}-{{slug}}` |
| activities（活動） | `content/activities` | `{{year}}-{{month}}-{{day}}-{{slug}}` |
| settings（網站設定） | `data/` | JSON file collection |

### 圖片管理

圖片依 collection 分別存放於 `images/` 下的對應子目錄：
- `images/faculty/` — 教師大頭照
- `images/publications/` — 論文封面圖
- `images/news/` — 新聞圖片
- `images/activities/` — 活動圖片

## 部署相關設定

### GitHub OAuth App

- 設定位置：https://github.com/organizations/ntujour/settings/applications
- Authorization callback URL：`https://api.netlify.com/auth/done`

### Netlify

- Site configuration → Access & security → OAuth → GitHub provider（需填 Client ID + Client Secret）
- 使用者管理：將 GitHub 帳號加為 repo collaborator（Write 權限）即可登入 CMS

詳細設定步驟見 `README.md`。

## 注意事項

- locale 設定為 `zh_Hant`（繁體中文介面）
- 所有欄位不區分中英文，英文網站另外處理
- faculty 的授課科目、研究專長、經歷、學歷、簡介皆為 markdown widget

## Pending Tasks (待辦事項)

> 此區塊記錄未完成的任務，Claude 啟動時自動載入。

（目前無待辦事項）

---
*Last updated: 2026-02-11*
