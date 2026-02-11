# Project Work Log

## 2026-02-10

- [23:20] 建立 Decap CMS 專案結構，含 admin/、content/（四子目錄）、images/（四子目錄）
- [23:20] 撰寫 admin/index.html（CMS 入口）與 admin/config.yml（四個 collections 完整定義）
- [23:20] 建立四個範例 Markdown 檔案：faculty、publications、news、activities 各一
- [23:20] 建立 CLAUDE.md 與 LOG.md
- [23:30] 從 webadmin 搬移現有內容：29 faculty、51 activities、16 news、1 publications MD 檔
- [23:30] 搬移 157 張圖片至 images/ 對應子目錄
- [23:30] 搬移 4 個 JSON 設定檔（banner/intro/navigation/site_settings）至 data/
- [23:30] 重寫 config.yml 匹配現有資料欄位結構，新增 JSON file collections
- [23:30] 批次更新 MD 檔中圖片路徑 /static/images/ → /images/
- [23:30] 啟用 local_backend: true 供本地測試

- [23:35] 移除所有中英文欄位區分（name_en、title_en、body_en、lang 等）
- [23:35] Faculty：experience/education 從 list-of-objects 轉為純文字欄位
- [23:35] News/Activities：移除 title_en、body_en、lang 欄位（67 個檔案）
- [23:35] JSON 設定檔合併中英文欄位（banner、site_settings、navigation）
- [23:40] Faculty：授課科目、研究專長、經歷、學歷、簡介改為 markdown widget
- [23:40] Faculty：排序欄位移至類別下方

## 2026-02-11

- [00:00] 部署至 GitHub：建立 ntujour/decap-cms repo 並 push
- [00:00] 啟用 GitHub Pages
- [00:10] 改用 Netlify 部署（GitHub OAuth 需要 Netlify 作為 OAuth proxy）
- [00:10] 設定 GitHub OAuth App（callback URL: api.netlify.com/auth/done）
- [00:10] 設定 Netlify OAuth provider（GitHub Client ID + Secret）
- [00:20] CMS 上線測試成功，可透過 GitHub 帳號登入
- [00:20] 建立 README.md（部署設定、OAuth 設定、使用者管理文件）
- [00:25] 更新 CLAUDE.md 與 LOG.md
- [10:30] 修復 GitHub OAuth 登入失敗問題：config.yml 補上 base_url（Netlify OAuth proxy 必要設定）
- [11:00] 從 Netlify 完全搬遷至 Cloudflare Pages
- [11:00] 新增 functions/api/auth.js、callback.js（Cloudflare Pages Functions 處理 GitHub OAuth）
- [11:00] config.yml 改為 base_url: decap-cms.pages.dev + auth_endpoint: /api/auth
- [11:00] 建立新的 GitHub OAuth App（callback URL 指向 Cloudflare Pages）
- [11:00] 設定 Cloudflare Pages 環境變數（GITHUB_CLIENT_ID、GITHUB_CLIENT_SECRET）
- [11:00] CMS 在 Cloudflare Pages 上線測試成功
- [11:30] 清理專案根目錄：移除 10 個搬遷暫存檔（含 92MB zip）、過時的 README.md，精簡 .gitignore
- [12:00] 新增 unpublish_date（預定下架日期）與 expired（已下架）欄位至 news、activities、publications
- [12:00] 統一 publications 欄位名稱：expiration_date → unpublish_date
- [12:00] 新增 GitHub Actions workflow（.github/workflows/check-expired.yml）每日自動標記過期內容
