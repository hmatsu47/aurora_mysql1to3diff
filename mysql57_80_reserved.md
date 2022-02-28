# Aurora MySQL v1（MySQL 5.6.10a）→ v3（MySQL 8.0.23 またはそれ以降）の間に追加された予約語一覧

| 予約語             | 修正対象？ | 追加      | 削除 |
| ------------------ | -------- | --------- | ---- |
| CUBE               | Yes      | 8.0.1 (R) |      |
| CUME_DIST          | Yes      | 8.0.2     |      |
| DENSE_RANK         | Yes      | 8.0.2     |      |
| EMPTY              | Yes      | 8.0.4     |      |
| EXCEPT             | Yes      | 8.0.?     |      |
| FIRST_VALUE        | Yes      | 8.0.2     |      |
| FUNCTION           | Yes      | 8.0.1 (R) |      |
| GENERATED          | Yes      | 5.7.6     |      |
| GROUPING           | Yes      | 8.0.1     |      |
| GROUPS             | Yes      | 8.0.2     |      |
| JSON_TABLE         | Yes      | 8.0.4     |      |
| LAG                | Yes      | 8.0.2     |      |
| LAST_VALUE         | Yes      | 8.0.2     |      |
| LATERAL            | Yes      | 8.0.14    |      |
| LEAD               | Yes      | 8.0.2     |      |
| NTH_VALUE          | Yes      | 8.0.2     |      |
| NTILE              | Yes      | 8.0.2     |      |
| OF                 | Yes      | 8.0.1     |      |
| OPTIMIZER_COSTS    | Yes      | 5.7.5     |      |
| OVER               | Yes      | 8.0.2     |      |
| PERCENT_RANK       | Yes      | 8.0.2     |      |
| RANK               | Yes      | 8.0.2     |      |
| RECURSIVE          | Yes      | 8.0.1     |      |
| ROW                | Yes      | 8.0.2 (R) |      |
| ROWS               | Yes      | 8.0.2 (R) |      |
| ROW_NUMBER         | Yes      | 8.0.2     |      |
| STORED             | Yes      | 5.7.6     |      |
| SYSTEM             | Yes      | 8.0.3     |      |
| VIRTUAL            | Yes      | 5.7.6     |      |
| WINDOW             | Yes      | 8.0.2     |      |

- **追加**：予約語として追加されたバージョン
  - (R) が付いているものは、このバージョンでキーワードから予約語に変わったことを示す
- 予約語ではないキーワードを列名等として使う場合は「`」で括る必要は無い