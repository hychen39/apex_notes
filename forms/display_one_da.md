---
description: 當選擇銀行轉帳時，顯示「銀行帳戶」欄位，隱藏「信用卡卡號」欄位。反之亦然。 [Dynamic Action]
---

# 依選項值 擇一顯示某個欄位 \[DA]

### 情境

當選擇特定付款方式時，顯示該方式的欄位。

<figure><img src="../.gitbook/assets/2025-08-13_07-12-19 (1).gif" alt=""><figcaption></figcaption></figure>

### 做法

使用 Dynamic Action 控制元件的 Show 及 Hide.&#x20;

以付款方式欄位的值是否為「信用卡」做為測試條件。

當條件為真時，顯示 「信用卡卡號」，隱藏 「銀行銀號」

當條件為假時，隱藏 「信用卡卡號」， 顯示  「銀行銀號」



### 設定

#### 1. Trigger event 的設定

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

Execution > Event Scope 的值設為 Static. 因為此欄位不會被部份頁面更新( Partial Page Refresh, PPR) 程序更新，在綁定這個欄位上的事件觸發器一直都有效。若欄位包含在部份頁面更新的範圍，那麼 Event Scope 的值就要選擇 Dynamic.  進一步可參考 [13.2.5 Defining Dynamic Action Event Scope](https://docs.oracle.com/en/database/oracle/apex/24.2/htmdb/managing-dynamic-actions.html#GUID-639B578E-17DA-43A1-B449-871460AA4700)

When > Item(s) 的屬性選擇要綁定 Trigger Event 的一個或多個 item.&#x20;

Client-side Condition 下設定事件觸發判斷條件。目前的設定: P2\_PAYMENT\_METHOD = 'card' 時觸發事件。注意 'card' 是 LOV 的回傳值，不是標籤值。

#### 2.  設定判斷條件為 「真」 時要執行的動作

可以一次執行一個或多個動作。

我們顯示 「信用卡卡號」 欄位，隱藏 「銀行帳號」 欄位

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

#### 3.  設定判斷條件為 「假」 時要執行的動作

我們隱藏 「信用卡卡號」 欄位，顯示 「銀行帳號」 欄位



