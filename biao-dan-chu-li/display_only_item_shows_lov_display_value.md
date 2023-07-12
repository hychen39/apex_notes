# 讓 Display Only Item 顯示 LOV 的 Display Value

## User Story

1. 在表單頁面，有一個 item 是 Select List type. 顯示的是 LOV 的 Display Value, 但 item 內儲存的是 Return Value.&#x20;
2. 當轉跳到另一個頁面，讓使用者確認表單填寫資訊。該頁面使用 Display Only item 顯示資料。該如何用 Display Only item 顯示 LOV 的 Display Value 而不是 Return Value ?&#x20;

![](<../.gitbook/assets/image (7).png>) ![](<../.gitbook/assets/image (22).png>)

## 原理

1. 設定 Display Only Item 的 List of Value 性，將某個 LOV 與此 item 勾稽
2. 設定 Display Only Item 的顯示值來自於 LOV 的 Display Value

## 準備工作

時段 LOV 在多個頁面重覆使用，所以將此 LOV 做成 Shared Component.&#x20;

此 LOV 的名稱命名為 PROID\_LIST

![](<../.gitbook/assets/image (1) (2).png>)

![](<../.gitbook/assets/image (21).png>)

## 步驟

S1 設定 Display Only item 的 List of Value 屬性：

* Type: Shared Component
* List of Values: PERIOD\_LIST

![](<../.gitbook/assets/image (4).png>)

S2 設定 Display Only item 的 Setting 屬性:&#x20;

* Based On: Display Value of List of Values

![](<../.gitbook/assets/image (9).png>)

S3 完成



