---
description: 載入頁面時，取得其它頁面的值，用它們來初始化表單欄位
---

# 載入頁面時，使用其它頁面的值初始化表單欄位

## 原理

在 render page 前，執行 PL/SQL 程式，從 Session State 取得其它頁面的 item 的值。之後，將這些值指派到目的頁面的欄位。

![](<../.gitbook/assets/image (20).png>)

## User Story

S1. 使用者在 Page 3 輸入空間預約表單

![](<../.gitbook/assets/image (3).png>)

S2. 按下確定後，要轉跳到 Page 4， 顯示方才輸入的資料，供使用者確認。

![](<../.gitbook/assets/image (6).png>)

## 設定步驟

S1. 在 Page 4 的 Before Region 節點，新增 Process

* 為 Process 設定 Name.
* 確認屬性&#x20;
  * Identification > Type: Execute Code
  * Source > Language: PL/SQL

![](<../.gitbook/assets/image (21) (1).png>)

S2. 在 Source > PL/SQL code 中輸入以下:

```plsql
begin

:P4_SAPCE := :P3_SPACE;
:P4_ITIME := :P3_TIME;
:P4_IDATE := :P3_DATE;
:P4_UID_1 := :P3_ID_1;
:P4_UID_2 := :P3_ID_2;

end;
```

Page Item 前使用冒號，表示要存取 Session State 中 Item 的值。

S3. 結束。

