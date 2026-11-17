# 大同大學課程查詢系統 — 自動化測試（Playwright MCP）

本 Repository 為 **HF Context Course — Unit 2 MCP** 課堂作業的測試成果存放區。

- 測試標的：https://couquery.ttu.edu.tw/main/ListClassCourse.php （公開課程查詢功能）
- 測試方式：OpenCode（MCP Host）+ `@playwright/mcp`（MCP Server）
- 測試日期：2026-11-17

## 目的

僅用於存放自動化測試發現的網站問題（Issue），**不對學校官方 Repository 提交任何內容**。
測試過程未登入任何帳號、未繞過存取限制、未進行大量自動化請求。

## 檔案

| 路徑 | 說明 |
|---|---|
| `evidence/TC01-page-load.png` | 網站載入測試畫面 |
| `evidence/TC02-TC03-query-result.png` | 查詢結果（19 筆） |
| `evidence/TC04-empty-result.png` | 查無資料時畫面空白（缺陷證據） |
| `evidence/TC05-dept-class-cascade.png` | 系所→班級連動測試 |
| `evidence/session.md` | 完整 MCP Tool Call 紀錄（自動產生） |
