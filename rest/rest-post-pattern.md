---
description: >-
  “說明如何處理 REST Endpoint 的 POST 請求, 包括自動變數的使用, 請求內容的處理, 回覆狀態碼及內容的設定, 以及範例程式碼.
  幫助開發人員在 Oracle ORDS 中處理 RESTful API 的 POST 請求。”
---

# RESTful Service 的 POST 請求處理範本

## 簡介

* 客戶端要提交資料給伺服器時, 通常會使用 HTTP POST 方法, 將資料提交到某個端點(endpoint)。
* 客戶端通知伺服器端上傳的資料型態(Content Type), 及資料內容。
* 伺服器端會根據請求的內容, 進行相應的處理, 並回覆客戶端。

## 自動變數 (Implicit Variables)

端點的處理器(handler)負責處理請求, 並回覆客戶端。

Oracle ORDS 會自動解析請求的內容, 放到變數中, 這些變數稱為自動變數(Implicit Variables)。

取得請求內容相關的自動變數:

* `:content_type`: 請求的內容類型, 例如 'application/json' 或 'text/plain'
* `:body`: 請求的主體內容, BLOB 型態. 當上傳的資料是二進位檔案時, 建議使用 `：body` 來取得請求的內容。
* `:body_text`: 請求的主體內容, CLOB 型態. 當上傳的資料是文字時, 如 JSON 或 XML, 建議使用 `：body_text` 來取得請求的內容。此變數在 ORDS 24.1 版本以上可用。

回覆請求的內容相關的自動變數:

* `:status_code`: 請求的狀態碼, 例如 200 或 404. 用於回覆客戶端

進一步細節參考 [3 Implicit Parameters, ORDS Developer's Guide](https://docs.oracle.com/en/database/oracle/oracle-rest-data-services/24.1/orddg/implicit-parameters.html#GUID-76A23568-EA67-4375-A4AA-880E1D160D27)

## 處理請求

### `:body` 與 `:body_text` 的使用注意事項

* `:body` 及 `:body_text` 在 PL/SQL 中只能使用一次。
  * 第一次讀取時, 會讀取請求的內容, 之後將 \`\`:body`及`:body\_text\` 請空。
  * 如果要多次使用, 必須將請求的內容存到變數中, 例如

```sql
declare
  l_body_text clob;
begin

    l_body_text := :body_text;
    -- 使用 l_body_text 變數
    -- ...
end;
```

* `:body` 及 `:body_text` 只能使用其中一個, 不能同時使用。
  * 同時使用時會產生錯誤, 並顯示訊息 "Duplicate steam parameter" (重複的蒸氣參數)

## 回覆請求處理狀態

### 設定狀態碼

設定 `:status_code` 變數, 來回覆請求的狀態碼。

典型的狀態碼有:

* 200: 請求成功
* 201: 資源已建立
* 204: 請求成功, 但沒有內容
* 400: 錯誤的請求
* 401: 未授權
* 403: 禁止訪問
* 404: 找不到資源
* 500: 伺服器錯誤

### 設定回覆內容

在 ORDS 中, 不用自己建立 HTTP 回覆的標頭(header)及內容(body), ORDS 會自動處理 HTTP 回覆的標頭及內容。

利用自動變數 `:status_code` 設定狀態碼

若要產生回覆內容(Response Body), 需要對應 JSON 欄位名稱及 PL/SQL 中的綁定變數

回覆內容的預設格式為 JSON 格式, 例如:

```sql
{
"message": "This is a custom field."
}
```

### 回覆欄位與 PL/SQL 綁定變數間的對應

在 handler 頁面的下方, Parameters 區域, 可以設定回覆欄位的名稱及 PL/SQL 綁定變數的對應關係。

* `Name`: 回覆欄位的名稱, 當 `Source Type` 設定為 `RESPONSE` 時; 如果 `Source Type` 設定為 `HTTP HEADER`, 則是請求的 HTTP Header 內的欄位名稱.
* `Bind Variable`: 在 PL/SQL 中的綁定變數名稱.
* `Access Method`: 設定 PL/SQL 綁定變數的存取方法, `IN`, `OUT` 或 `IN/OUT`.
  * 如果參數只從 HTTP Header 讀取, 則設定為 `IN`.
  * 如果參數只回覆給客戶端, 則設定為 `OUT`.
  * 如果參數同時從 HTTP Header 讀取, 並回覆給客戶端, 則設定為 `IN/OUT`.
* 在 `Source Type`: 設定參數的來源, 可以選擇 `RESPONSE` 或 `HTTP HEADER`。
* `Data Type`: 設定參數的資料型態.

### 範例

如果在 Response Body 中要有 `message` 欄位回覆給客戶端, 其 Parameter 的設定:

| Name    | Bind Variable | Access Method | Source Type | Data Type |
| ------- | ------------- | ------------- | ----------- | --------- |
| message | msg           | OUT           | RESPONSE    | String    |

在 PL/SQL 中, 設定回傳訊息的內容:

```sql
begin
    :msg := 'This is a custom field.';
    :status_code := 200;
end;
```

在客戶端收到的回覆內容:

```json
{
  "message": "This is a custom field."
}
```

## POST 請求處理的範本(Pattern)

### 前置設定

在 POST handler 中, 設定需要的在 Response Body 中的回覆欄位, 及 PL/SQL 綁定變數的對應關係.

### 程序

1. 取得請求的內容
2. 必要時, 將 blob 轉成 clob
3. 呼叫你的程序
   * 程序要在外部先測試過, 確定可以正常運作
   * 把程序寫成 package function/procedure, 方便管理及測試
   * 不在 handler 中測試, 非常不容易
4. 設定回覆的欄位的值, 例如:錯誤訊息
5. 設定狀態碼

### 處理範本

```sql
declare
-- 取得 Request Body 的內容, binary
l_body_blob blob;
-- 取得 Request Body 的內容, text
l_body_text clob;
-- 存放 Content-Type
l_content_type varchar2(200);

begin

-- Get the request body from the implicit binding variable :body
l_body_blob := :body;

-- 或者取得 Text 的內容, :body 或 :body_text 只能兩者擇一, 並且只能使用一次
-- l_body_text := :body_text;

-- Get the content type
l_content_type := :content_type;


-- 如果需要轉換 blob 內容為 clob, 可以使用 apex_util.blob_to_clob
-- 這個函數可避免 buffer overflow 的問題
-- Use apex_util.blob_to_clob
-- l_body_text := apex_util.blob_to_clob(l_body_blob);

-- 檢查內容是否為空, 如果是, 回傳錯誤訊息給客戶端
if l_body_blob is null then
    :message := 'Request body is empty';
    :status_code := 400;
    return; 
end if;



-- 
-- call your procedure/function or package. 
--


-- 設定狀態碼
:status_code := 200;  
:message := 'SUCCESS';


end;
```
