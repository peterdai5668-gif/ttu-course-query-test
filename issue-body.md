# [Bug] 課程查詢系統查無資料時未顯示任何提示訊息，頁面呈現空白

## 問題描述

大同大學課程查詢系統（各班級課程查詢）在「查無資料」的情況下，會直接不渲染結果表格，且**未顯示任何「查無資料」或錯誤提示**。使用者只會看到查詢表單下方一片空白，無法判斷是：

- 該班級本學期沒有課程，或
- 查詢條件有誤，或
- 系統發生錯誤／仍在載入中

此為可用性（UX）缺陷，可能造成使用者重複操作或誤判系統故障。

## 環境

| 項目 | 內容 |
|---|---|
| 頁面 | https://couquery.ttu.edu.tw/main/ListClassCourse.php |
| 瀏覽器 | Google Chrome（Playwright 驅動，viewport 1440x900） |
| 測試日期 | 2026-11-17 |
| 測試方式 | Playwright MCP（AI Agent 自動化測試） |

## 重現步驟

1. 開啟 https://couquery.ttu.edu.tw/main/ListClassCourse.php
2. 於「班級課程」的第一個下拉選單選擇 **I:資工系所**
3. 於第二個下拉選單選擇 **資訊工程學系七年級B班**（value = `UI7B`）
4. 觀察結果表格區域

## 預期結果

頁面應顯示明確的空狀態提示，例如「查無資料」、「此班級本學期無課程資料」或類似訊息，讓使用者知道查詢已成功執行但無符合資料。

## 實際結果

- 結果表格元素**完全不存在**（`document.querySelector('table')` 回傳 `null`）
- 頁面全文（去除空白後僅 446 字元）**不含**「無資料／查無／沒有資料／no data／not found」任一關鍵字
- 畫面僅剩查詢表單與頁尾，結果區域一片空白

### 對照組（有資料時正常）

| 查詢條件 | 結果筆數 |
|---|---|
| I:資工系所 / 二年級A班（UI2A） | 19 筆 |
| E:電機系所 / 一年級A班 | 25 筆 |
| I:資工系所 / 七年級B班（UI7B） | 0 筆，且**無任何提示** |

## 影響範圍

- 使用者無法區分「無資料」與「系統異常」
- 對自動化測試與無障礙（a11y）工具而言，缺少狀態訊息會使結果無法被正確判讀
- 建議於後端回傳空集合時，前端顯示 `<div class="empty-state">查無資料</div>` 或對應的無障礙提示（`role="status"`）

## 相關截圖

| 檔案 | 說明 |
|---|---|
| `https://raw.githubusercontent.com/peterdai5668-gif/ttu-course-query-test/main/evidence/TC04-empty-result.png` | 查無資料時的空白畫面 |
| `https://raw.githubusercontent.com/peterdai5668-gif/ttu-course-query-test/main/evidence/TC02-TC03-query-result.png` | 對照組：有資料時正常顯示 19 筆 |
| `evidence/page-2026-11-17T10-19-16-796Z.yml` | 頁面 Accessibility Snapshot（正常狀態結構） |
| `evidence/session-1794910751752/session.md` | 完整 MCP Tool Call 執行紀錄 |

## 備註

本 Issue 來自課堂作業之自動化測試，僅針對**公開課程查詢功能**進行測試，未嘗試登入、繞過存取限制或進行大量請求。此 Issue 建立於個人測試 Repository，未向學校官方 Repository 提交。

---

*Detected by: OpenCode (MCP Host) + Playwright MCP Server (`@playwright/mcp`), Test Case TC04*

