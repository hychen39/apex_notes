---
description: 介紹如何處理 JSON Object，使用 Oracle 的 JSON_TABLE 函數解析 JSON 資料並將其轉換為關聯式資料表格式。
---

# 處理 JSON 格式的資料

## 概述

客戶端的 POST 請求會傳送 JSON Object 到後端.\
後端需要解析 JSON Object, 以便進行後續的處理, 例如新增資料到資料表中。

Oracle 提供了 `JSON_TABLE` 函數來解析 JSON Object, 將其轉換為關聯式資料表的格式, 可做為 FROM 子句的資料來源。

## 使用 JSON\_TABLE 函數解析 JSON Object

以下範例展示如何使用 `JSON_TABLE` 函數解析 JSON Object，並將其內容存入 PL/SQL 的記錄變數中。

```sql
DECLARE
    l_data clob := q'! {
  "日期": "2025/1/1",
  "支付方式": "現金",
  "類別": "食",
  "支出": 1500,
  "收入": null,
  "備註": "餐廳聚餐"
} !';

    type txn_rec_type is record (
        txn_date date,
        payment_method varchar2(20),
        category varchar2(20),
        expense number,
        income number,
        note varchar2(100)
    );

    l_rec txn_rec_type;

BEGIN
    SELECT * INTO l_rec
    FROM JSON_TABLE(
        l_data,
        '$'
        COLUMNS (
            txn_date VARCHAR2(20) PATH '$.日期',
            payment_method VARCHAR2(20) PATH '$.支付方式',
            category VARCHAR2(20) PATH '$.類別',
            expense NUMBER PATH '$.支出',
            income NUMBER PATH '$.收入',
            note VARCHAR2(100) PATH '$.備註'
        )
    );

    DBMS_OUTPUT.PUT_LINE('日期: ' || l_rec.txn_date);
    DBMS_OUTPUT.PUT_LINE('支付方式: ' || l_rec.payment_method);
    DBMS_OUTPUT.PUT_LINE('類別: ' || l_rec.category);
    DBMS_OUTPUT.PUT_LINE('支出: ' || l_rec.expense);
    DBMS_OUTPUT.PUT_LINE('收入: ' || l_rec.income);
    DBMS_OUTPUT.PUT_LINE('備註: ' || l_rec.note);
END;
/
```

### 說明

1. **JSON Object 定義**: 使用 `CLOB` 型別定義 JSON 資料。
2. **記錄型別定義**: 使用 `RECORD` 型別定義資料結構。
3. **JSON\_TABLE 函數**:
   * `l_data` 是 JSON 資料來源。
   * `$` 表示 JSON 的根路徑。
   * `COLUMNS` 子句定義了每個欄位的名稱、型別及其對應的 JSON 路徑。
4. **輸出結果**: 使用 `DBMS_OUTPUT.PUT_LINE` 輸出解析後的資料。

## JSON\_TABLE() 的語法

基本語法：

```sql
JSON_TABLE(
    json_source,       -- JSON 資料來源，可以是 CLOB、BLOB 或 VARCHAR2
    json_path          -- JSON 路徑，用於指定要解析的 JSON 節點
    COLUMNS (          -- 定義要轉換的 table 欄位 及對應的 JSON 路徑
        column_name data_type PATH 'json_path' [DEFAULT expr] [ERROR ON ERROR | NULL ON ERROR],
        ...
    )
)
```

### 語法說明

1. **`json_source`**: JSON 資料來源，可以是 CLOB、BLOB 或 VARCHAR2。
2. **`json_path`**: JSON 路徑，用於指定要解析的 JSON 節點，通常以 `$` 表示根路徑。
3. **`COLUMNS` 子句**: 定義輸出的欄位，包括：
   * **`column_name`**: 欄位名稱。
   * **`data_type`**: 欄位的資料型別，例如 `VARCHAR2`、`NUMBER` 等。
   * **`PATH 'json_path'`**: 指定欄位對應的 JSON 路徑。
   * **`DEFAULT expr`**: 指定欄位的預設值（可選）。
   * **`ERROR ON ERROR | NULL ON ERROR`**: 指定當解析失敗時的行為（可選）。

### 範例

```sql
SELECT *
FROM JSON_TABLE(
    '{"name": "John", "age": 30}',
    '$'
    COLUMNS (
        name VARCHAR2(50) PATH '$.name',
        age NUMBER PATH '$.age'
    )
);
```

此範例將 JSON 資料解析為兩個欄位：`name` 和 `age`，並將其轉換為關聯式資料表格式。

## 將 JSON Object Insert 到資料表中

使用 INSERT SELECT 可直接將 JSON Object 插入到資料表中。

語法:

```sql
INSERT INTO your_table (column1, column2, ...)
SELECT column1, column2, ...
FROM JSON_TABLE(
    l_data,
    '$'
    COLUMNS (
        column1 VARCHAR2(20) PATH '$.key1',
        column2 NUMBER PATH '$.key2'
    )
);
```

將上述的例子改寫成 INSERT SELECT。\
假設目的表格 txn\_table 已經存在，並且有對應的欄位。

```sql
DECLARE
    l_data clob := q'! {
  "日期": "2025/1/1",
  "支付方式": "現金",
  "類別": "食",
  "支出": 1500,
  "收入": null,
  "備註": "餐廳聚餐"
} !';

BEGIN
    insert into txn_table (txn_date, payment_method, category, expense, income, note)
    SELECT txn_date, payment_method, category, expense, income, note
    FROM JSON_TABLE(
        l_data,
        '$'
        COLUMNS (
            txn_date VARCHAR2(20) PATH '$.日期',
            payment_method VARCHAR2(20) PATH '$.支付方式',
            category VARCHAR2(20) PATH '$.類別',
            expense NUMBER PATH '$.支出',
            income NUMBER PATH '$.收入',
            note VARCHAR2(100) PATH '$.備註'
        )
    );
    COMMIT;
end;
/

```

這樣的程式碼更為簡潔，並且可以直接將 JSON Object 插入到資料表中。

## 參考資料

* [JSON\_TABLE, SQL Language Reference, R23](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/JSON_TABLE.html)
