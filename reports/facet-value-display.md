# 設定 facet 中顯示不重覆的選項值

## 問題描述

建立 Facet Search 頁面後, 某個 facet 中的選項值出現眾多的重覆值(如下圖)。為什麼會這樣? 又該如何處理呢?&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### 原理

Facet 區域中顯示的是某個 LOV 元件的內的可用值。每個 Facet 都會勾稽到一個 LOV.&#x20;

在下圖中，P2\_ORG\_TYPE 是 Facet 區域下的一個 item, 其 List Of Values 屬性設定使用 CG.TYPE 的 LOV 元件。所以，我們只要設定 CT.TYPE 回傳正確的，不重覆的值，就可以解決問題。&#x20;

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### 步驟

#### 1. 編輯 Facet 相對應的 LOV 元件

以 P2\_TARGET\_GROUP 為例，其對應的 LOV 元件為 CG.TARGET\_GROUP.&#x20;

在 Share Component 的 LOV 可找到此元件，接著進行編輯。

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

#### 2. 撰寫 LOV 的 SQL, 回傳 unique values

進 CG.TARGET\_GROUP 的編輯後，目前的 Source Type 為 Table。所以，回傳的資料列有重覆值。

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

&#x20;改用 SQL Query 的 Source Type。撰寫以下 SQL:

```sql
select distinct target_group from A5_CHARITY_GROUP
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

套用變更。

再執行頁面就可看到正確的結果。

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
