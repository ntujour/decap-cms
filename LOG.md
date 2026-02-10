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

## 2026-02-10

- [--:--] Transformed all 29 faculty markdown files: converted experience/education from list-of-objects (zh/en) to plain text block scalars (zh only), removed name_en and title_en fields. Script: transform_faculty.py
