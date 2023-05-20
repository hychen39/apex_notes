---
description: 按下按鈕後, 顯示認對話框, 詢問是否執行動作.
cover: .gitbook/assets/image (7).png
coverY: -136
---

# 按下按鈕後, 顯示確認對話框

## 情境

在表單輸入後，按下 (B)Create，系統提示確認對話框。

![](<.gitbook/assets/image (7).png>)

## 原理

1. 按下 (B)Create, 系統執行 Javascript 顯示對話框
2. 系統執行 `apex.page.confirm()` 或者 `apex.confirm()` 顯示對話框
3. 按下(B)OK 後，Apex 執行 form 的 `Form - Automatic Row Processing (DML)` process 將表單的欄位資料新增到表格中。&#x20;

![](<.gitbook/assets/image (11).png>)

4. (B)Create 的 Behavior 的 Database Action 屬性會決定做那種 DML 操作

![](<.gitbook/assets/image (2).png>)

## 設定步驟

### 前題

已使用 Form Wizard 建立，為 Table 建立 Form&#x20;

### 步驟

Step 1. 檢視表單的 (B)Create 的 Behavior 屬性區

Step 2. 修改 Action 成為 `Redirect to URL`&#x20;

Step 3. 設定 Target 屬性:

```
javascript:apex.confirm('確定儲存？','CREATE');
```

參數 1: pMessage, 提示訊息文字

參數 2: pOptions, 呼叫選項。提供 `CREATE` 告知 Apex 請求的動作類型。

詳細說明參考：

* [apex.confirm JS api](https://docs.oracle.com/en/database/oracle/apex/22.2/aexjs/apex.page.html#.confirm)
* [About Calling JavaScript from a Button](https://docs.oracle.com/database/apex-18.1/HTMDB/creating-buttons.htm#GUID-DCA5A309-B58B-4655-A488-5C7505BD1ACE)
* [About the Relationship Between Button Names and REQUEST](https://docs.oracle.com/database/apex-18.1/HTMDB/creating-buttons.htm#GUID-D300F08C-C93C-47AE-9304-E8DF41797226)

完成的設定

![](<.gitbook/assets/image (10).png>)

Step 4. 完成
