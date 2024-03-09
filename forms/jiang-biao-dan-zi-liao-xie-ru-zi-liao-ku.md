---
description: >-
  將 Form Region 的資料寫入到資料庫。使用 Form - Automatic Row Processing (DML) 程序自動產生 Insert
  statement 新增資料至 table 中。
---

# 將表單資料寫入資料庫

## 情境

使用者已輸入資料至表單。按下(B)確定後，系統將資料存入表格

## 原理

頁面的 Region 的型態要使用 Form。在 Form Region 放置 Item UI 元件，使用者可輸入資料

Form Region 的資料來源為 Table。

該 Region 下的 Item 的資料來源是 Region 的 Table 下的 column.&#x20;

在頁面加入 Form Region 時, Apex 會自動增加 Form - Automatic Row Processing (DML) 到頁面中

* 需要指定該程序要處理的 Form Region.&#x20;
* 執行時，此程序會視觸發按鈕的行為(database action), 產生相對應的 DML statememt.

新增按鈕到頁面, 設定按鈕的 Behavior 相關屬性:

* Action: Submit Page
* Database Action: SQL INSERT action

## 程序

### 準備

1. 先建立所需要的表格及欄位

* 表格 `HY_DIS_RESERVATIONS`

![](<../.gitbook/assets/image (71).png>)

2. 建立空白頁面

## 步驟

S1. 新增 Region 到頁面，將 Region 的型態設為 `Form`

S2. 設定此 Form Region 的 Source 屬性：

* Table Name 為目的表格名稱 `HY_DIS_RESERVATIONS`

![](<../.gitbook/assets/image (72).png>)

S3. 設定完成後，Apex 會自動為 Table 內的每個 Column 新增相對應的 Item 到 Form Region 中。

![](<../.gitbook/assets/image (8).png>)

S4. 指定 Form Region 中做為 Primary Key 的 Item.&#x20;

![](<../.gitbook/assets/image (23).png>)&#x20;

S5. 新增按鈕到頁面。設定按鈕的 Behavior 相關屬性:

![](<../.gitbook/assets/image (44).png>)

S6. 設定 Form - Automatic Row Processing (DML) 的屬性:

* Form Region 屬性為 S3 的 Region 名稱。&#x20;
* 此 process 於 Submit Page 後執行。

![](<../.gitbook/assets/image (24).png>)



S7. 提交表單後, 此 Process 會產生 INSERT statement 並執行。

![](<../.gitbook/assets/image (31).png>)

S8. 完成



