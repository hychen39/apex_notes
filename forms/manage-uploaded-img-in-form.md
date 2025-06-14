---
description: 在表單中可一次上傳多張照片。之後，在表單中使用 Interactive Report (IR) 顯示上傳的圖片，並在 IR  中提供 刪除 的連結，以便刪除先前上傳的圖片。
---

# 在 IR 中顯示上傳的圖片並提供刪除連結

## Use Case

在表單中可一次上傳多張照片. 

如果表單中的主鍵是空值，不要顯示圖片上傳的區域。
因為，圖片上傳時，除了有自己的主鍵外, 還會將主表的主鍵當成外鍵存入圖片表中。如此，才能顯示特定表單的圖片。

使用 IR  顯示屬於該表單的目前己上傳的圖片。

在 IR  中有一 action column 提供刪除圖片的連結。

## 步驟

S1. 檢視成品、目前半成品頁面及測試資料。

檢視成品、目前半成品頁面及測試資料。
說明 IR 顯示圖片的技術原理。
我們使用 Display Image 元件顯示圖片。
設定 IR Region 中的圖片欄位使用 Display Image 元件
設定 Display Image 元件的相關的 BLOB 屬性。

[https://youtu.be/ZPz1UwRZ9nc](https://youtu.be/ZPz1UwRZ9nc)

S2. 撰寫 IR Region 的 SQL 查詢.

Image Display 的 SQL column 必須要傳回個大於 0 的數字。
使用 dbms_blob.getlength() 函數來取得圖片的長度。

注意, 修改 Query 後, 先前的 Display Image column 的設定會重新設定。所以順序上, 先撰寫 SQL 查詢, 再設定 Display Image column。

[https://youtu.be/NTwbzMwS04E](https://youtu.be/NTwbzMwS04E)

S3. 在 IR 上顯示 Action Column。

Action Column 是一個 Link 元件。
設定 Action Column  的 Target 及 Link Attributes。

在 Link Attributes 會加入 `class="del_img_act"`. 之後使用此 CSS  class 來掛 Dynamic Action。

[https://youtu.be/07CyKAcPrrc](https://youtu.be/07CyKAcPrrc)

S4. 為有 `del_img_act` class 的 Action Column 按鈕掛 Dynamic Action。此步驟會有三個子步驟。

S4-1 在控制台中顯示取得的 `<a>` 元素的 `data-id` 屬性值。

[https://youtu.be/kCgleASWGU0](https://youtu.be/kCgleASWGU0)

建立第一個 Dynamic Action (DA) true action，使用 JavaScript 取得所有有 `del_img_act` class 的 `<a>` 元素，並在控制台中顯示其 `data-id` 屬性值。

S4-2 將取得的圖片 ID 存入 page item。

建立第二個 DA true action，使用 JavaScript 取得 `<a>` 元素的 `data-id` 屬性值，並將其存入 page item。

[https://youtu.be/w2sRj-R72WE](https://youtu.be/w2sRj-R72WE)

S4-3 撰寫 PL/SQL 程式碼，刪除圖片。

建立第三個 DA true action，使用 PL/SQL 程式碼刪除圖片。

[https://youtu.be/wvJUetNe5BU](https://youtu.be/wvJUetNe5BU)

S5 依據 form 的主鍵值，顯示圖片上傳區域。

如果主鍵是空值，不顯示圖片上傳區域。
使用 Dynamic Action 來依據主鍵值顯示或隱藏圖片上傳區域。

[https://youtu.be/gMpvl-tnaCY](https://youtu.be/gMpvl-tnaCY)


