# Aurora MySQL v1（MySQL 5.6.10a）→ v3（MySQL 8.0.23 またはそれ以降）の間に動作が変わった機能（前述のものを除く）一覧

## 最適化（オプティマイザ）

- 空間インデックス
  - Aurora MySQL v1 独自機能で InnoDB に実装、Aurora MySQL v3 で本家 MySQL のものに置き替えられた
  - SRID=0 ではオプティマイザに無視されるように
    - https://dev.mysql.com/doc/refman/8.0/ja/spatial-index-optimization.html
    - https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/AuroraUserGuide/Aurora.AuroraMySQL.Overview.html#Aurora.AuroraMySQL.Spatial
    - https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.MySQL80.html#AuroraMySQL.mysql80-spatial

## 言語構造（キーワードと予約語を除く）

- `CHECK`制約の有効化
  - 過去に無視された制約の定義が有効あるいはエラーに
    - https://dev.mysql.com/doc/refman/8.0/ja/alter-table.html#alter-table-foreign-key
- `SELECT`・`UNION`パーサールールの変更
  - https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-8-0-versus-5-7
    - ロック句を含む `SELECT`ステートメントにはカッコが必要に

## 文字セット・照合順序

- `utf8mb4`のデフォルト照合順序が`utf8mb4_0900_ai_ci`へ
  - その他、フォールバックのルールなど
    - https://dev.mysql.com/doc/refman/8.0/ja/charset-connection.html

## データ型

| データ型 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `YEAR(2)` | 5.7.5 (R) | 廃止→ `YEAR(4)`へ https://dev.mysql.com/doc/refman/5.7/en/migrating-from-year2.html |
| `YEAR(4)` | 8.0.19 (R) | 非推奨→ `YEAR`へ https://dev.mysql.com/doc/refman/8.0/ja/date-and-time-type-syntax.html |
| 文字列データ型の`BINARY`属性 | 8.0.17 (D) | 非推奨 https://dev.mysql.com/doc/refman/8.0/ja/string-type-syntax.html 8.0.28 で`ASCII`・`UNICODE`も非推奨に https://dev.mysql.com/doc/refman/8.0/en/string-type-syntax.html |

## SQL ステートメントなど

| データ型 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `CHANGE MASTER TO` | 8.0.23 (D) | `CHANGE REPLICATION SOURCE TO`へ https://dev.mysql.com/doc/refman/8.0/ja/change-replication-source-to.html |
| `CREATE TEMPORARY TABLE`での`TABLESPACE = {innodb_file_per_table \| innodb_temporary}` | 8.0.13 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/create-temporary-table.html |
| `GRANT` | 8.0.? (R) | 暗黙のユーザ作成およびユーザ属性のみの変更を廃止 https://dev.mysql.com/doc/refman/8.0/ja/mysql-nutshell.html#mysql-nutshell-removals |
| `GROUP BY ASC/DESC` | 8.0.13 (R) | 廃止 https://dev.mysql.com/doc/refman/8.0/ja/upgrading-from-previous-series.html#upgrade-sql-changes 8.0.12 でグループ化関数を使用した`ORDER BY`サポート |
| `INSERT DELAYED` | 5.7.? (R) | 廃止 https://dev.mysql.com/doc/refman/5.7/en/insert-delayed.html InnoDB では元から使えない |
| `ORDER BY 【列番号】` | 8.0.? (D) | 非推奨 https://dev.mysql.com/doc/refman/8.0/ja/select.html カッコで囲まれたクエリー式内で発生し外部クエリーにも適用される`ORDER BY`（動作不定）も非推奨に |
| `REPLACE DELAYED` | 5.7.? (R) | 廃止 https://dev.mysql.com/doc/refman/5.7/en/replace.html InnoDB では元から使えない |
| `RESET SLAVE` | 8.0.22 (D) | `RESET REPLICA`へ https://dev.mysql.com/doc/refman/8.0/ja/reset-replica.html |
| `SHOW ENGINE INNODB MUTEX` | 5.7.2 (R) → 5.7.8 | 一旦廃止後再導入（仕様変更に注意） https://dev.mysql.com/doc/refman/5.7/en/show-engine.html |
| `SHOW GRANTS` | 8.0.? | 動的権限導入により`ALL PRIVILEGES`が表示されなくなった https://dev.mysql.com/doc/refman/8.0/ja/show-grants.html |
| `SHOW SLAVE STATUS` | 8.0.22 (D) | `SHOW REPLICA STATUS`へ https://dev.mysql.com/doc/refman/8.0/ja/show-replica-status.html |
| `SQL_CALC_FOUND_ROWS` | 8.0.17 (D) | 非推奨 https://dev.mysql.com/doc/refman/8.0/ja/select.html |
| `START SLAVE` | 8.0.22 (D) | `START REPLICA`へ https://dev.mysql.com/doc/refman/8.0/ja/start-replica.html |
| `TABLE ... UNION (TABLE)`の挙動の変更 | 8.0.19 | https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-table |
| `UNION DISTINCT`・`UNION ALL`の挙動の変更 | 8.0.19 | https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-distinct-all |
| `UNION`の制限変更 | 8.0.20 | https://dev.mysql.com/doc/refman/8.0/ja/union.html#union-restrictions |

## その他

- 64 文字を超える外部キー制約名を持つテーブルは NG に
- Connector を対応バージョンに入れ替え
  - https://dev.mysql.com/doc/refman/8.0/ja/connectors-apis.html
- `CREATE TABLE ... SELECT`のトランザクションの扱いが変更（8.0.21）
  - 行ベースレプリケーションで 1 つのトランザクションとして記録
    - https://dev.mysql.com/doc/refman/8.0/ja/replication-features-create-select.html
- GTID レプリケーションの非互換
  -  https://dev.mysql.com/doc/refman/8.0/ja/replication-options-gtids.html
- `GRANT`操作の読み取りロックの変更（8.0.22）
  - https://dev.mysql.com/doc/refman/8.0/ja/grant-tables.html#grant-tables-concurrency
- `LOCK TABLES ... WRITE`による明示的テーブルロック時の`innodb_table_locks=0`無効化
  - トリガーなどによる暗黙的テーブルロック時は有効
- SQL モードの非推奨・削除
  - 5.6.17 で非推奨、5.7 で厳密モードの動作の一部に（個別に`ERROR_FOR_DIVISION_BY_ZERO`を指定しても動作が変わらないように）
    - `ERROR_FOR_DIVISION_BY_ZERO`
  - 5.7.22 で非推奨、8.0 で削除
    - `DB2`, `MAXDB`, `MSSQL`, `MYSQL323`, `MYSQL40`, `ORACLE`, `POSTGRESQL`, `NO_FIELD_OPTIONS`, `NO_KEY_OPTIONS`, `NO_TABLE_OPTIONS`
      - https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html#sql-mode-changes
  - 8.0 で削除
    - `NO_AUTO_CREATE_USER`
      - MySQL のデフォルト（≠ Aurora MySQL のデフォルト）からも削除（`GRANT`での暗黙のユーザ作成が廃止されたため）
      - https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html#sqlmode_no_auto_create_user
  - 8.0.13 で非推奨
    - `PAD_CHAR_TO_FULL_LENGTH`
      - https://dev.mysql.com/doc/refman/8.0/ja/sql-mode.html#sql-mode-full
- アトミック DDL 導入によるレプリケーションの挙動変化
  - `IF EXISTS`が付かない`DROP TABLE`・`DROP VIEW`のレプリケーション差異
    - https://dev.mysql.com/doc/refman/8.0/ja/atomic-ddl.html#atomic-ddl-statement-behavior
- デフォルト認証が変わったことにより、Aurora MySQL v3 で新規ユーザを作成した場合に既存アプリケーションから接続できない可能性がある
  - `CREATE USER`時に`mysql_native_password`を指定する
    - https://dev.mysql.com/doc/refman/8.0/ja/create-user.html#create-user-overview
- パーティショニングの実装を変更
  - 基本的には Aurora MySQL での実質的な非互換は無い
    - https://dev.mysql.com/doc/refman/8.0/ja/partitioning-overview.html
  - `KEY`パーティショニングのカラムインデックス接頭辞が非推奨に（8.0.21）
    - https://dev.mysql.com/doc/refman/8.0/ja/partitioning-limitations.html#partitioning-limitations-prefixes
- プリペアドステートメント内のユーザー変数への参照のタイプは、ステートメントが最初に準備されたときに決定され、それ以降にステートメントが実行されるたびにこのタイプが保持されるように（8.0.22）
  - https://dev.mysql.com/doc/refman/8.0/ja/user-variables.html
- レプリケーション設定の不整合（ギャップ）にかかわる設定項目の変更（8.0.19）
  - https://dev.mysql.com/doc/refman/8.0/ja/replication-features-transaction-inconsistencies.html
  - https://dev.mysql.com/doc/refman/8.0/en/replication-features-transaction-inconsistencies.html （8.0.26 以降の変更）
- 一時テーブルの`ROW`・`MIXED`形式バイナリログ記録の変更（5.7.25・8.0.4）
  - https://dev.mysql.com/doc/refman/8.0/ja/replication-rbr-usage.html#replication-rbr-usage-temptables
- 管理者権限の分割（Aurora MySQL v1 → v3 変更点でもピックアップ）
  - https://dev.mysql.com/doc/refman/8.0/ja/privileges-provided.html
- 個々の ENUM または SET カラム要素の長さが 255 文字または 1020 バイトを超えるテーブルまたはストアドプロシージャは NG に
  - 以前は ENUM または SET のカラム要素の最大長 64K
- 内部一時テーブルの変更（Aurora MySQL v1 → v3 変更点でもピックアップ）
  - https://dev.mysql.com/doc/refman/8.0/ja/internal-temporary-tables.html
  - https://dev.mysql.com/doc/refman/8.0/en/internal-temporary-tables.html （8.0.27 以降の変更）
- 明示的に定義されたカラム名が 64 文字を超えるビューは NG に
  - 以前は 255 文字まで許可
