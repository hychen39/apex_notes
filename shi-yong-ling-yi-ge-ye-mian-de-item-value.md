---
description: 把 item 的 value 存到 Session State 中，以進行不同頁面(app page)間的資料分享
---

# 使用另一個頁面的 item value

## 情境

使用者在 page 1 輸入資料後，按下確定，轉跳另一個頁面(page 2)，顯示先前輸入的資料，供者用者確認。



## 原理

* Apex 為每個 Session 建立一個 Session State, 在 Server 端儲存頁面上 item 的值。
  * Session State 是由 Apex Server 管理
* 在頁面上輸入時，輸入值尚在前端(browser)的元件上，尚未送到 Server 端\[1]。
* 提交(submit) 頁面後，元件上的值才會進到 Session State.&#x20;
  * Item Value 指 Item 在 Session State 上的值
  * Field value 指 UI Component 上的值
* 要存取 Session State 中的值，依情境(context)有不同的語法
  * 在 SQL 或 PL/SQL 下，可使用 bind variable syntax&#x20;
    * `:page_item`
    * [參考 3.8.4 Referencing Session State Using Bind Variable Syntax](https://docs.oracle.com/en/database/oracle/apex/22.2/htmdb/managing-session-state-values.html#GUID-A052DC04-0D04-4DEA-BDEF-4DCBFF489E07)
  * 在非程式的環境下，如在 Static Region 以 HTML 顯示 Item Value, 使用 Substitution Strings
    * 在 直接顯示文字的地方，使用 `&ITEM.` syntax. `&` 為開始符號，`.` 結束符號
      * 在 interactive grid, cards page, or map 元件中也是使用這個語法
    * 若要參考 Report 上的 column, 則要使用 `#column#` syntax
    *   若要跳脫 Item Value 中的某些特殊字元，參考 [3.9.2 Controlling Output Escaping in Substitution Strings](https://docs.oracle.com/en/database/oracle/apex/22.2/htmdb/using-substitution-strings.html#GUID-CA3ABA44-D03D-4396-A527-B160F1FFE933)



## 程序

### 前題

* Page 1 輸入資料
* Page 2 顯示先前輸入的資料

### 步驟

Step 1. 設定 Page 1 上的 Item Component 的 Maintain Session State 為 Per Session (Disk)

![](<.gitbook/assets/image (11) (1).png>)

Step 2. 設定"確定"按鈕的 Behavior 為 Submit Page.&#x20;

如果使用 Redirect, 元件上的值不會送到 Server 端，Field value 不會存到 Session State 中。

![](<.gitbook/assets/image (6) (1).png>)

Step 3. 設定 Submit page 後轉跳至 Page 2.&#x20;

在提交後處理的 After Processing 階段，建立 Branch. 轉跳到 Page 2

![](<.gitbook/assets/image (12) (1).png>)

Step 4. 在 Page 2 中，設定 Item Component 的資料來源為 Session State 中的 Item Value

* Item 屬性為目的 item 元件的資料來源，是 page 1 的 item.&#x20;
* Used 屬性改用 Always. 如此，當轉跳至此頁面時，便會用新的值覆寫目的 item 的 value.&#x20;

![](<.gitbook/assets/image (10) (1).png>)

Step 5. 完成。



## References

1. [3.8.2 About Setting Session State](https://docs.oracle.com/en/database/oracle/apex/22.2/htmdb/managing-session-state-values.html#GUID-48081ADE-FADA-43C6-A273-EB30694360DF), App Builder User's Guide 22.2



