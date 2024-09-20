---
description: The programming pattern for the REST POST handler in PL/SQL
---

# Post handler pattern

```plsql
declare
  -- get the body content from the request
  l_body_blob blob := :body; 
  l_body_clob clob;
  l_response clob := 'post handler';
begin
  -- Cast the body of the blob type to the clob type.
  l_body_clob := UTL_RAW.cast_to_varchar2(l_body_blob);
  
  -- Call your functions to do tasks. 
  
  
  -- response code
  :status := 200;
  -- custom response message
  htp.p(l_response);
end;
```
