---
description: 在 Region 中建立一個 Popup LOV 元件； 值清單的資料來自於 table.
---

# 建立一個 Popup LOV 元件

Step 1. 加入 Item 至 Region

Step 2. 設定 Item Type 為 Popup LOV

![](<.gitbook/assets/image (4) (1).png>)

Step 3. 設定清單值的型態為 SQL Quer

* (Property)List of Value > Type
* ![](<.gitbook/assets/image (3).png>)

Step 4. 在 SQL Query 中輸入查詢

* 第一個欄位為顯示值欄位
* 第二個欄位為回傳值欄位

```sql
select DEPT.DNAME as display_val,
    DEPT.DEPTNO as return_val 
 from DEPT DEPT
```

![](<.gitbook/assets/image (7).png>)

Step 5. 執行，便可看到 Popup LOV

![](<.gitbook/assets/image (8).png>)



Step 6. 預設為 inline 顯示清單值。若想使用強制式對話框(Modal Dialog), 設定 (P)Settings > Displays 屬性。

![](<.gitbook/assets/image (5).png>)

