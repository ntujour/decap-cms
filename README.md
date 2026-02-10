# Decap CMS — 台大新聞所內容管理系統

Decap CMS 管理介面，用於維護台大新聞所網站的師資、學生發表、最新消息、活動等內容。

CMS 網址：https://monumental-cuchufli-928165.netlify.app/admin/

---

## 架構概述

```
使用者 → Netlify (hosting + OAuth proxy) → GitHub API → ntujour/decap-cms repo
```

- **內容儲存**：GitHub repo（`ntujour/decap-cms`），以 Markdown + YAML frontmatter 格式
- **CMS 介面**：Decap CMS，部署在 Netlify
- **認證方式**：GitHub OAuth，透過 Netlify 代理
- **圖片儲存**：repo 內 `images/` 目錄

---

## 部署設定紀錄

### 1. GitHub OAuth App

在 GitHub 建立 OAuth App，讓使用者能透過 GitHub 帳號登入 CMS。

**設定位置**：https://github.com/organizations/ntujour/settings/applications

| 欄位 | 值 |
|------|-----|
| Application name | `Decap CMS` |
| Homepage URL | `https://monumental-cuchufli-928165.netlify.app/` |
| Authorization callback URL | `https://api.netlify.com/auth/done` |

> callback URL **必須**是 `https://api.netlify.com/auth/done`，這是 Netlify OAuth proxy 的固定端點。

### 2. Netlify 站台

Netlify 負責兩件事：hosting 靜態檔案、作為 GitHub OAuth 的中介 proxy。

**建立方式**：

1. 到 [app.netlify.com](https://app.netlify.com) → Add new site → Import an existing project
2. 選 GitHub → `ntujour/decap-cms`
3. Build 設定全部留空（純靜態，不需 build）→ Deploy

**OAuth Provider 設定**：

1. Site configuration → Access & security → OAuth
2. Install provider → 選 GitHub
3. 填入 GitHub OAuth App 的 **Client ID** 和 **Client Secret**

### 3. Decap CMS Config（`admin/config.yml`）

```yaml
backend:
  name: github
  repo: ntujour/decap-cms
  branch: main
```

- `name: github`：使用 GitHub backend，認證透過 Netlify OAuth proxy
- 不需要 `git-gateway`、不需要 Netlify Identity

---

## 使用者管理

### 新增使用者

只要將對方的 GitHub 帳號加為 repo 的 collaborator：

1. 到 https://github.com/ntujour/decap-cms/settings/access
2. 點 **Add people**
3. 輸入對方的 GitHub 帳號，權限選 **Write**
4. 對方接受邀請後即可登入 CMS

如果對方已是 `ntujour` organization member 且有 repo write 權限，則不需額外設定。

### 移除使用者

到同一個頁面，移除 collaborator 即可。

---

## 本地開發測試

本地測試不需要 GitHub 登入，使用 Decap CMS 的 local backend：

1. 在 `admin/config.yml` 最上方加一行：

   ```yaml
   local_backend: true
   ```

2. 開兩個 terminal：

   ```bash
   # Terminal 1：Decap local proxy server
   npx decap-server

   # Terminal 2：靜態伺服器
   python3 -m http.server 8000
   ```

3. 開啟 http://localhost:8000/admin/

> 記得 push 前把 `local_backend: true` 移除。

---

## 內容結構

| Collection | 資料夾 | 說明 |
|------------|--------|------|
| 師資 | `content/faculty/` | 教師資料 |
| 學生發表 | `content/publications/` | 論文、研究發表 |
| 最新消息 | `content/news/` | 系所公告、榮譽、招生等 |
| 活動 | `content/activities/` | 講座、工作坊、研討會等 |
| 網站設定 | `data/*.json` | 橫幅、簡介、導覽列、網站資訊 |

圖片依 collection 分別存放於 `images/` 下的對應子目錄。

---

## 常見問題

### 登入時顯示 "Not Found"

- 確認 Netlify 已設定 GitHub OAuth provider（Site configuration → Access & security → OAuth）
- 確認 GitHub OAuth App 的 callback URL 是 `https://api.netlify.com/auth/done`

### 登入時顯示 "redirect_uri is not associated with this application"

- GitHub OAuth App 的 Authorization callback URL 不正確，改為 `https://api.netlify.com/auth/done`

### 本地測試時要求登入

- 確認 `admin/config.yml` 最上方有 `local_backend: true`
- 確認 `npx decap-server` 有在執行
