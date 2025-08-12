---
description: >-
  在使用預設身份鑑別機制通過鑑別後，因為要切換身份，所以需要使用另一種鑑別方式。例如，原本使用 Open Door Auth，之後要因要切換成
  Admin，所以需使用 Apex Account auth 鑑別身份。
---

# 登入後，切換另一種身份鑑別方式

## User Story

App 的預設鑑別方式為 Open Door, 使用者只需輸入帳號名稱即可，不用密碼驗證。

登入後，使用者想切換到管理員身份，所要需要使用 Apex Account Auth，以取後身份。



故事實現

1. 先使用 Open Door 登入系統

![](<../.gitbook/assets/image (94).png>)

2. 在 Nav Toolbar 上提供 Admin Login 按鈕。按下後，轉跳畫面，告知即將登出目前身份。

![](<../.gitbook/assets/image (95).png>)

![](<../.gitbook/assets/image (96).png>)

3. 使用者按下 (B)OK 後，登出目前身份，並改用 Apex Account Auth. 系統轉跳至帳號/密碼輸入頁面。

![](<../.gitbook/assets/image (97).png>)

4. 成功後改用新的身份。

![](<../.gitbook/assets/image (98).png>)



## 原理

1. 一但使用某種身份鑑別機制，之要再切換到另一種，需要先登出之後再鑑別
2. 非預設的 Auth Scheme 要啟用  `Switch in Session 功能。`
3. ``在未登入系統的狀態下，使用請求參數 `APEX_AUTHENTICATION=auth_scheme_name` 切換到未預設的 `auth_scheme_name` auth scheme 執行身份鑑別。``

## 設定步驟

### Task 1. 將 App 的預設 Auth Schem 改成 Open Door&#x20;

1. Path: App Home > Shared Components > Authentication Schemes&#x20;
2. (B)Create&#x20;
3. 選擇  "Based on a pre-configured scheme from the gallery"

![](<../.gitbook/assets/image (101).png>)

4. 設定 Name，及 Scheme Type
5. (B)\[Create Authentication Scheme]

![](<../.gitbook/assets/image (102).png>)

6. 完成後，App 預設使用 Open Door Auth Scheme

### Task 2. 修改 Apex Account Scheme, 並啟用 `Switch in Session 功能。`

1. 回到 Authentication Schemes 頁面。點選 Apex Express Accounts。
2. 修改 Apex Express Accounts scheme 的名稱為 `apex`&#x20;

![](<../.gitbook/assets/image (103).png>)

3. 點選 ``Login Processing tab. 設定 `Switch Session` 為 Enable. 之後 (B)[Apply Changes]``

![](<../.gitbook/assets/image (104).png>)

### `Task 3. 製作 "告知登出" 頁面。`

1. 建立 new page. Page Mode 設為 Modal Dialog.\
   確認頁面需要身份鑑別: \
   ![](<../.gitbook/assets/image (111).png>)
2. 加入一個 Static Content Region 至 page body.&#x20;
3. 在 Region 的 Source > Text 撰寫告知訊息
4. 加入 Cancel button, 放置 Page template 的 CLOSE position; 加入 OK button, 放置到 CREATE position.&#x20;

![](<../.gitbook/assets/image (105).png>)

5. 切換至 Processing tab
6. 加入第一個 procss, 當使用者按下 (B)OK 時登出目前身份。\
   執行的 PL/SQL Code:

```plsql
apex_authentication.logout(:SESSION, :APP_ID);
```

![](<../.gitbook/assets/image (106).png>)

5. 加入第二個 process, 當使用者按下 (B)Cancel 時，關閉對話框。

![](<../.gitbook/assets/image (107).png>)

6. 在 After Processing 節點，加入 Branch, 鑑別成功後，轉跳 Page 1 (Home)\
   設定 branch target 時，Request 欄位的值要設定成 \`APEX\_AUTHENTICATION=apex`，其中` apex 是要使用的非預設的 Auth Scheme.&#x20;

![](<../.gitbook/assets/image (108).png>)

![](<../.gitbook/assets/image (109).png>)

### Task 4 在 Navigation Bar 加入 Admin Login 按鈕

1. Nav Path: App Home > Shared Components > Navigation > Navigation Bar List
2. 點擊 "Desktop Navigation Bar"\
   ![](<../.gitbook/assets/image (88).png>)
3. (B)Create 建新 entry.
4. 輸入 List Entry Label 及 Target Page.\
   Target Page 為在 Task 3 建立的 page. \
   ![](<../.gitbook/assets/image (89).png>)

### Task 5 測試

1. 先使用 Open Door 身份鑑別登入 App.
2. 按下 Nav Toolbard 上的 (B)\[Admin Login], 應出現 "告知登出" 的對話框。\
   ![](<../.gitbook/assets/image (90).png>)\
   ![](<../.gitbook/assets/image (91).png>)
3. 點擊 (B)Cancel 應會關閉對話框；點擊 (B)OK 應會轉到身份鑑別畫面：\
   ![](<../.gitbook/assets/image (92).png>)
4. 成功登入後，轉挑到 Page 1. \
   ![](<../.gitbook/assets/image (93).png>)

{% hint style="info" %}
2023/10/03 released
{% endhint %}

