---
description: 使用 Cards List 做功能導覽
---

# Cards List 導覽

想要在頁面上使用 Cards List 做功能導覽

![](<../.gitbook/assets/image (70).png>)

### 原理

內容為以 List 儲放； 使用 Cards List template 顯示 List 內的內容

### 步驟

Step 1. 建立一個 Static List

* Path：app> Shared Components > Lists > (B)Create

Step 2. 建立 List 內的內容。

* 每個 list entry 可以設定：List Entry Label, Image/Class, 及 Target

![](<../.gitbook/assets/image (75).png>)

Step 3. 在目的頁面中，新增一個 Region, 並設定此 Region 的屬性。

* Type: List
* Attributes > Apperrance > List Template: Cards

![](<../.gitbook/assets/image (57).png>)

Step 4. 設定 Template Options.

* Apply Template Colors: check
* Icons: Display Icons

![](<../.gitbook/assets/image (1) (1).png>)

Step 5. 儲存頁面，並執行。即可完成

![](<../.gitbook/assets/image (6).png>)
