---
description: 有開始和結束時間兩個欄位。這兩個欄位決定的時間間隔最多只能兩個小時。違反規則時，在表單上顯示錯誤訊息，表單資料不會 submit。
---

# 表單欄位檢查: 時間最多 2 小時

## User Story

有兩個 LOV 欄位，預約開始與結束時間。可預約的最長時間為 2 個小時。預約時段以 30 分鐘為單位。

預約開始與結束時間欄位皆顯示所有的時段。

提交表單時，檢查結束時間欄位。若預約的最長時間超 2 個小時，顯示錯誤訊息。

例如，開始時間為 08:00, 結束時間為 10:30， 此時違反規則，要顯示錯誤訊息。



## 原理

在結束時間欄位設置 Validation, 於 submit 表單時檢查。

每個 Validation 必須要設定的屬性：

* Validation 相關屬性: 設定檢查資料的邏輯
* Error 相關屬性: 設定錯誤訊息內容及顯示的位置

時段以流水號編列，例以 1 表 08:00，2 表 08:30，以此略推。使用時段間的差距計算時間間隔。

例如08:00開始(id: 1), 10:30(id: 6), 6 - 1 ＝ 5 > 4 表示超出 2 小時。

## 資料集準備

參考 [串接 LOV](../lov/cascade_lov.md#zi-liao-ji-zhun-bei)&#x20;

## 步驟

S1 在表單上建立開始與結束時間 2 個 LOV 欄位。

![](<../.gitbook/assets/image (58).png>)

S2 這兩個 LOV 欄位的 List of Values 的來源為 Shared Component.

![](<../.gitbook/assets/image (4) (1).png>)

HY\_PERIOD\_LIST 內的 Query 如下:

![](<../.gitbook/assets/image (10).png>)

S3 在 P6\_END\_TIME\_1 欄位新增一個 Validation.

![](<../.gitbook/assets/image (27).png>)

S4 接著設定檢查的邏輯

使用的 type 為 PL/SQL Expression. 運算後的結果需為 TRUE or FALSE。

在 PL/SQL Expression 輸入以下的算式：

```plsql
:P6_END_TIME_2 - :P6_START_TIME_1 <= 4
```

算式運算的結果為 TRUE or FALSE.&#x20;

![](<../.gitbook/assets/image (65).png>)

![](<../.gitbook/assets/image (9).png>)

S5 設定錯誤訊息內容及顯示位置

Display Location 屬性選擇 Inline with Field and in Notification. \
錯誤訊息會顯示在兩個地方，一個為關聯欄位(Associated Item) 下方，另一個為右上方的通知區域。

![](<../.gitbook/assets/image (33).png>)

![](<../.gitbook/assets/image (18).png>)

S6 在表單中增加一個 Region. 設定 Appearance > Template 屬性時用 Buttons Container.

![](<../.gitbook/assets/image (15).png>)

![](<../.gitbook/assets/image (61).png>)

此外，在此 Region 中新增一個 Button, 以便提交表單. Button 的 Behavior > Action 設為 Submit Page.

![](<../.gitbook/assets/image (87).png>)

S7 測試。若時間間隔超過 2 小時，會在錯誤欄位下方及右上方顯示錯誤訊息。

![](<../.gitbook/assets/image (63).png>)
