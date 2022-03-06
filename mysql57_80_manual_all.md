# Aurora MySQL v1（MySQL 5.6.10a）→ v3（MySQL 8.0.23 またはそれ以降）の間に動作が変わった機能（前述のものを除く）一覧

## 最適化（オプティマイザ）


## 言語構造（キーワードと予約語を除く）


## 文字セット・照合順序


## データ型

| データ型 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `DATE(2)` | 5.7.5 (R) | 廃止→ `DATE(4)` へ https://dev.mysql.com/doc/refman/5.7/en/migrating-from-year2.html |

## SQL ステートメントなど

| データ型 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `INSERT DELAYED` | 5.7.? (R) | 廃止 https://dev.mysql.com/doc/refman/5.7/en/insert-delayed.html |
| `REPLACE DELAYED` | 5.7.? (R) | 廃止 https://dev.mysql.com/doc/refman/5.7/en/replace.html |
| `SHOW ENGINE INNODB MUTEX` | 5.7.2 (R) → 5.7.8 | 一旦廃止後再導入（仕様変更に注意） https://dev.mysql.com/doc/refman/5.7/en/show-engine.html |

## その他
