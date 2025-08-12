---
description: 使用 Chart UI 元件，以長條圖顯示資料
cover: ../.gitbook/assets/image (52).png
coverY: -166
---

# 長條圖顯示資料

## 情境

顯示各城市下的各部門的員工人數

城市為數列; 部門為 label; 人數為 Value

![](<../.gitbook/assets/image (52).png>)

## 原理

1. 使用 Chart Region 顯示圖形

* 在 Chart Region 的 Attributes 中可以選擇圖形的型態，例如 bar, line 等。

![](<../.gitbook/assets/image (3) (1).png>)

* 可在以 Legend 屬性區中設定是否顯示圖例(Legend)

![](<../.gitbook/assets/image (84).png>)

2. 圖形的資料來源為 SQL Query&#x20;

![](<../.gitbook/assets/image (49).png>)

3. 需要對應 Query Column 到 圖形元件的 Label, Value 等屬性以顯示資料。

![](<../.gitbook/assets/image (16).png>)

## 程序

### 前題

先安裝 `HR Data` sample dataset.\
Path: (M)\[SQL Workshop] > Utilities > \[Sample Datasets]

![](<../.gitbook/assets/image (11).png>)

### 步驟

S1. 在新的 page 上，加入 Chart Region 到頁面的 Content Body&#x20;

S2. 設定 Chart Region 的 Title.

S3. 設定 Region 的圖形型態

* Attributes > Chart > Type = Bar

![](<../.gitbook/assets/image (17).png>)

S4. 啟用 Legend&#x20;

* Attributes > Legend > Show = Enable

![](<../.gitbook/assets/image (68).png>)

S5. 設定 Series 的名稱

* 建立 Chart Region, Apex 會自動新增一個 Series.

S6. 設定 Series 的資料來源

* Source type: SQL Query
* SQL Query

````sql
```sql
select l.city, d.department_name, count(e.employee_id)
from oehr_employees e natural join oehr_departments d
     natural join oehr_locations l
group by l.city, d.department_name
order by 2;
```
````

![](<../.gitbook/assets/image (32).png>)

S7. 設定 Query 欄位與 Chart Region 的屬性的對應

![](<../.gitbook/assets/image (13).png>)

S8. 完成



