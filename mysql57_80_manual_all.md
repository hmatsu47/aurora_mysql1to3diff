# Aurora MySQL v1（MySQL 5.6.10a）→ v3（MySQL 8.0.23 またはそれ以降）の間に動作が変わった機能（前述のものを除く）一覧

## 最適化（オプティマイザ）


## 言語構造（キーワードと予約語を除く）

- `SELECT`・`UNION`パーサールールの変更
  - https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-8-0-versus-5-7
    - ロック句を含む `SELECT`ステートメントにはカッコが必要に
- `CHECK`制約の有効化
  - 過去に無視された制約の定義が有効あるいはエラーに
    - https://dev.mysql.com/doc/refman/8.0/ja/alter-table.html#alter-table-foreign-key

## 文字セット・照合順序

- `utf8mb4`のデフォルト照合順序が`utf8mb4_0900_ai_ci`へ
  - その他、フォールバックのルールなど
    - https://dev.mysql.com/doc/refman/8.0/ja/charset-connection.html

## データ型

| データ型 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `DATE(2)` | 5.7.5 (R) | 廃止→ `DATE(4)` へ https://dev.mysql.com/doc/refman/5.7/en/migrating-from-year2.html |

## SQL ステートメントなど

| データ型 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `CHANGE MASTER TO` | 8.0.23 | `CHANGE REPLICATION SOURCE TO`へ https://dev.mysql.com/doc/refman/8.0/ja/change-replication-source-to.html |
| `CREATE TEMPORARY TABLE`での`TABLESPACE = {innodb_file_per_table \| innodb_temporary}` | 8.0.13 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/create-temporary-table.html |
| `GROUP BY ASC/DESC` | 8.0.13 (R) | 廃止 https://dev.mysql.com/doc/refman/8.0/ja/upgrading-from-previous-series.html#upgrade-sql-changes |
| `INSERT DELAYED` | 5.7.? (R) | 廃止 https://dev.mysql.com/doc/refman/5.7/en/insert-delayed.html |
| `REPLACE DELAYED` | 5.7.? (R) | 廃止 https://dev.mysql.com/doc/refman/5.7/en/replace.html |
| `SHOW ENGINE INNODB MUTEX` | 5.7.2 (R) → 5.7.8 | 一旦廃止後再導入（仕様変更に注意） https://dev.mysql.com/doc/refman/5.7/en/show-engine.html |
| `SHOW SLAVE STATUS` | 8.0.22 | `SHOW REPLICA STATUS`へ https://dev.mysql.com/doc/refman/8.0/ja/show-replica-status.html |
| `START SLAVE` | 8.0.22 | `START REPLICA`へ https://dev.mysql.com/doc/refman/8.0/ja/start-replica.html |
| `TABLE ... UNION (TABLE)`の挙動の変更 | 8.0.19 | https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-table |
| `UNION DISTINCT`・`UNION ALL`の挙動の変更 | 8.0.19 | https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-distinct-all |
| `UNION`の制限変更 | 8.0.20 | https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-restrictions |

## その他

- 64 文字を超える外部キー制約名を持つテーブルは NG に
- 明示的に定義されたカラム名が 64 文字を超えるビューは NG に
  - 以前は 255 文字まで許可
- 個々の ENUM または SET カラム要素の長さが 255 文字または 1020 バイトを超えるテーブルまたはストアドプロシージャは NG に
  - 以前は ENUM または SET のカラム要素の最大長 64K
- アトミック DDL 導入によるレプリケーションの挙動変化
  - `IF EXISTS`が付かない`DROP TABLE`・`DROP VIEW`のレプリケーション差異
    - https://dev.mysql.com/doc/refman/8.0/ja/atomic-ddl.html#atomic-ddl-statement-behavior
- `CREATE TABLE ... SELECT`のトランザクションの扱いが変更（8.0.21）
  - 行ベースレプリケーションで 1 つのトランザクションとして記録
- パーティショニングの実装を変更
  - Aurora MySQL での実質的な非互換は無し
    - https://dev.mysql.com/doc/refman/8.0/ja/partitioning-overview.html
- デフォルト認証が変わったことにより、Aurora MySQL v3 で新規ユーザを作成した場合に既存アプリケーションから接続できない可能性がある
  - `CREATE USER`時に`mysql_native_password`を指定する
    - https://dev.mysql.com/doc/refman/8.0/ja/create-user.html#create-user-overview
- 管理者権限の分割（Aurora MySQL v1 → v3 変更点でもピックアップ）
  - https://dev.mysql.com/doc/refman/8.0/ja/privileges-provided.html
- GTID レプリケーションの非互換
  -  https://dev.mysql.com/doc/refman/8.0/ja/replication-options-gtids.html
- Connector を対応バージョンに入れ替え
  - https://dev.mysql.com/doc/refman/8.0/ja/connectors-apis.html
- `GRANT`操作の読み取りロックの変更（8.0.22）
  - https://dev.mysql.com/doc/refman/8.0/ja/grant-tables.html#grant-tables-concurrency