---
description: 內建文字訊息只有提供英文訊息。當使用非英文語系時，文字訊系仍以英文訊息顯示。本篇說明如何自訂不同語系下的內建文字訊息。
---

# 修改不同語系下的內建文字訊息

## User Story

當違反必要欄位驗證規則時，期望顯示中文的錯誤訊息:

![](<../.gitbook/assets/image (6).png>)

## 原理

Apex 內建的文字訊息以 key-value 的方式儲存。例如：違反必要欄位驗證規則時的文字訊息的 key 為 `APEX.PAGE_ITEM_IS_REQUIRED`，value 為 `#LABEL# must have some value`.

完整的 internal messages 請參考 [Table 21-3 Internal Messages Requiring Translation.](https://docs.oracle.com/en/database/oracle/application-express/19.1/htmdb/translating-messages.html#GUID-52E139BF-73E1-46AB-8A4E-E046865CEA2E)

到 App > Shared Components > Text Messages 去翻譯英文的文字訊息。

![](<../.gitbook/assets/image (53).png>)

Name 欄位: 文字訊息的 Key 值

Text 欄位: 翻譯的內容。
