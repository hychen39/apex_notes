---
description: 當選擇銀行轉帳時，顯示「銀行帳戶」欄位，隱藏「信用卡卡號」欄位。反之亦然。
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

1. Trigger event 的設定

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>
