---
description: >-
  允許 App 使用多種身份鑑別方式(authentication scheme), 預設為 Apex Express Auth. 但提供 Google
  Login 按鈕，使用者可切換用 Google 帳號登入。
cover: ../.gitbook/assets/login.jpg
coverY: 165.275
---

# 登入時切換不同的身份鑑別方式

## User Story

在首頁提供 Google Login 的按鈕，

![](<../.gitbook/assets/image (24).png>)

或者在 Login page 提供

![](<../.gitbook/assets/image (27).png>)

## 原理

App 中有多種的身份鑑別方式。

登入時可切換到非預設的身份鑑別方式。

例如，App 的預設身份鑑別方式為 Apex Express Account. 但若按下 Google Login 按鈕時，要切換到 Google\_Auth 方式。

![](<../.gitbook/assets/image (29).png>)

切換到另一個 auth scheme 要具備三個條件:

* 目的頁面為 private (Requiring Authentication)
* 該 auth scheme 允許 `Switch in Session`
* Redirect 到目的頁面時，使用  `APEX_AUTHENTICATION` 請求參數(request parameter)指定要使用的非預設鑑別方式。

## 前題(pre-requisitions)

App 完成 Google Login 的 Authentication Scheme 的設定。[請參考 Apex 應用程式的 Google Social Account Sign-in 設定](https://hychen39.github.io/oracle\_apex/2023/05/23/apex\_social\_signin.html).

假設目前有兩個 Authentication Scheme: `Apex` (default) 及 `Google_Auth`:

![](<../.gitbook/assets/image (23).png>)

## 步驟

S1 建立一個空白 page (page 3)，確認其 Security > Authentication 屬性為 `Page Requires Authentication`.

S2 在 page 的 Before Header 執行點，新增一 Branch，轉跳到 App 的首頁(例如 page 1)

![](<../.gitbook/assets/image (30).png>)

S3 啟用 `Google_Auth scheme` 的 `Switch in Session` 屬性

瀏覽 App Home > Shared Components  > Authentication Schemes 編輯 `Google_Auth` scheme.

設定 Login Processing > Switch in Session 為 Enabled

![](<../.gitbook/assets/image (21).png>)

點擊 ? 有詳細的說明:

![](<../.gitbook/assets/image (20).png>)

S4 在 App Home page (page 1) 新增一個按鈕，提供 Google account login.

該按鈕按下後要轉跳先前建立的 page (page 3).

轉跳時要設定請求參數 `APEX_AUTHENTICATION=Google_Auth`.&#x20;

![](<../.gitbook/assets/image (26).png>)

S5 測試.&#x20;

點擊 Google Login 按鈕後，會轉跳到 Page 3. 因為 Page 3 是 private, 所以會啟用身份鑑別。

請求參數 `APEX_AUTHENTICATION=Google_Auth` 指定使用 Google\_Auth 方案，所以會使用 Google Account 進行鑑別。

成功後，轉到 Page 3. Page 3 載入時會再轉跳到 Page 1.&#x20;

![](<../.gitbook/assets/image (19).png>)

![](<../.gitbook/assets/image (25).png>)

![](<../.gitbook/assets/image (28).png>)

