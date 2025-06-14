# oracle apex 處理多對多關係的建立

## 任務

一個志工(volunteer)可以參加多個活動(event). volunteer 和 event 之間是多對多的數量關係。

我們想要在寫入志工資料到表格時，一併建立起和事件之間的關係。我們需要一併將 event\_id 和 volunteer\_id 兩個鍵值寫到 volunteer 和 event 間的 join table.



資料表間的關係如下:

<figure><img src=".gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

## Apex 的設定

{% embed url="https://www.youtube.com/watch?v=9FFrNOuW1GY" %}
