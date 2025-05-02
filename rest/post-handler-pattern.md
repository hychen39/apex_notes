---
description: The programming pattern for the REST POST handler in PL/SQL
---

# Note 

新版的 POST handler 的說明可參考 [示範: 處理 REST API POST 請求](rest/rest-post-demo.md)。

# Post handler pattern

```sql
declare

l_body_blob blob;
l_base64_str clob := 'empty';
l_body_excerpt varchar2(500) := 'empty';

l_content_type varchar2(200);
l_response clob;
l_sid varchar2(100);
l_filename varchar2(4000);
l_err_payload json_object_t := new json_object_t();

begin

-- Get the request body from the implicit binding variable :body
l_body_blob := :body;

-- Get the content type
l_content_type := :content_type;

-- Convert the body's data type from blob to clob
-- trap of cast_to_varchar2: the varchar2 buffer is limited
-- l_base64_str := UTL_RAW.cast_to_varchar2(l_body_blob);

-- Suggest use apex_util.blob_to_clob to avoid buffer overflow 
-- Use apex_util.blob_to_clob
-- l_base64_str := apex_util.blob_to_clob(l_body_blob);
-- l_body_excerpt := DBMS_LOB.SUBSTR(l_base64_str, 50, 1);


if l_body_blob is null then
    raise_application_error(-20001, 'body is null');
end if;

/*
if l_body_excerpt is null then
    raise_application_error(-20002, 'l_body_excerpt is null');
end if;
*/


-- call your functions 
--
-- 

:status_code := 200;
:payload := l_response;
:message := 'SUCCESS';

-- Exception handling
EXCEPTION
    when others then
      :status_code := 400;
      :message := sqlerrm;
      l_err_payload.put('sid', :sid);
      l_err_payload.put('filename', :filename);
      -- l_err_payload.put('excerpt', l_body_excerpt);
      :payload := l_err_payload.to_string();

end;
```


