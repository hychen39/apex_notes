---
description: 讓使用者選擇清單上的某個值做為輸入值；有些元件(item) 可以輸入清單以外的值。
---

# 值清單 (List of Value)元件介紹

## 相關的頁面元件(Page Item)

* Text-Based 元件，某些提供 List of Value (LOV) 按鈕
  * 可以選擇 LOV 內的值或者自行輸入
* List-Based 元件限制可輸入的值，只能從提供的清單內選擇
  * 單選清單(Single-select list): 一次只能選擇一個
  * 複選清單(Multiple-select list): 可選擇多個

具 LOV 的 Text-based 元件

* Text field with autocomplete：邊輸入，可以列出相符的值

單選清單

* Popup LOV : 適合長清單；會另外顯示對話框，使用者可以搜尋清單值。
* Select List:  適合短清單；提供下拉清單，從中挑選;
* Radio Groups：單選的選項按鈕

複選清單

* Select List (啟用 Allow Multi Selection 屬性) 可參考 [Working with Multi Select List](https://blogs.ontoorsolutions.com/post/working-with-multi-select-list/)
* Checkbox Group
* List Manager&#x20;
* Shuttle

## LOV 的資料來源

清單值的欄位

* 顯示值(display value)： 使用者看到的值
* 回傳值(return value)：選擇後，儲存到元件(item)的值

清單值的來源：

* Static (靜態): Developer 提供顯示值(display value)及回傳值(return value)
* Dynamic(動態): 從 Table 查詢後，回傳 顯示值(display value) 及 回傳值(return value) 兩個欄位

