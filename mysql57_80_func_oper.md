# Aurora MySQL v1（MySQL 5.6.10a）→ v3（MySQL 8.0.23 またはそれ以降）の間に動作が変わったビルトイン関数・オペレータ一覧

- **[オペレータ](#オペレータ)**
- **[ビルトイン関数](#ビルトイン関数)**

---

**バージョン表記の後ろの記号について：**
- (D) : 非推奨（Deprecated）
- (R) : 削除・廃止（Removed）
- (NR) : Aurora MySQL v3 の将来のリリースで変更の可能性あり（MySQL 8.0.24 以降の変更点）

## オペレータ

| オペレータ | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `&` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `&&` | 8.0.17 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/logical-operators.html#operator_and |
| `>>` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `<<` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `^` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `\|` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `\|\|` | 8.0.17 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/logical-operators.html#operator_or **注記**も参照（`OR`とは別の使い方←非推奨にはならない） |
| `~` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `!` | 8.0.17 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/logical-operators.html#operator_not |
| `BINARY` | 8.0.27 (NR,D) | 非推奨に（`CAST(... AS BINARY)`に置換） https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#operator_binary |
| `NOT REGEXP`, `REGEXP`, `RLIKE` | 8.0.4 | 正規表現ライブラリが ICU に変わった影響を受ける可能性がある https://dev.mysql.com/doc/refman/8.0/ja/regexp.html#regexp-compatibility |

## ビルトイン関数

| 関数 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `ADDTIME()` | 8.0.28 (NR) | 戻り値の型を決める方法を変更 https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_addtime |
| `Area()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `AsBinary()`, `AsWKB()`, `AsText()`, `AsWKT()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `BIT_AND()` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `BIT_OR()` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `BIT_XOR()` | 8.0.0 | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `Buffer()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `CAST()` | 5.7.6 8.0.0 ほか | Spatial な型への変換について GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `Centroid()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `Contains()` | 5.7.6 (D) 8.0.0 (R) | `MBR` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `CONVERT()` | 5.7.6 8.0.0 ほか | Spatial な型への変換について GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `CONVERT_TZ()` | 8.0.28 (NR) | 64 ビット環境では最大値が `'3001-01-18 23:59:59.999999' UTC` に https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_convert-tz |
| `Crosses()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `DATE_ADD()`, `DATE_SUB()` | 8.0.22 to 8.0.28 (NR) | プリペアドステートメントを使う場合、引数での型の指定に関わらず`DATETIME`型の値を返す https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-add https://bugs.mysql.com/bug.php?id=103781 ※8.0.28 で元の動作に戻る |
| `DECODE()`, `ENCODE()` | 5.7.2 (D) 8.0.3 (R) | 非推奨→削除 `AES_ENCRYPT()`, `AES_DECRYPT()`への切り替え（注：非互換）を推奨 https://dev.mysql.com/doc/refman/5.7/en/encryption-functions.html#function_decode |
| `DES_DECRYPT()`, `DES_ENCRYPT()` | 5.7.6 (D) 8.0.3 (R) | 非推奨→削除 `AES_ENCRYPT()`, `AES_DECRYPT()`への切り替え（注：非互換）を推奨 https://dev.mysql.com/doc/refman/5.7/en/encryption-functions.html#function_des-decrypt |
| `Dimension()`, `Distance()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `Disjoint()` | 5.7.6 (D) 8.0.0 (R) | `MBR` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `ENCRYPT()` | 5.7.6 (D) 8.0.3 (R) | 非推奨→削除 `SHA2()`でのハッシュ化への切り替え（注：非互換）を推奨 https://dev.mysql.com/doc/refman/5.7/en/encryption-functions.html#function_encrypt |
| `EndPoint()`, `Envelope` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `Equals()` | 5.7.6 (D) 8.0.0 (R) | `MBR` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `ExteriorRing()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `ExtractValue()` |  | XML 関数は開発中の機能なので随時修正が入る https://dev.mysql.com/doc/refman/8.0/ja/xml-functions.html |
| `FOUND_ROWS()` | 8.0.17 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/information-functions.html#function_found-rows |
| `FROM_UNIXTIME()` | 8.0.28 (NR) | 64 ビット環境では最大値が `32536771199.999999`（`'3001-01-18 23:59:59.999999' UTC`） に https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_from-unixtime |
| `GeomCollFromText()`, `GeometryCollectionFromText()`, `GeomCollFromWKB()`, `GeometryCollectionFromWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `GeometryCollection()` | 5.7.6 8.0.0 ほか | GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `GeometryN()`, `GeometryType()`, `GeomFromText()`, `GeometryFromText()`, `GeomFromWKB()`, `GeometryFromWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `GLength()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `GREATEST()` | 8.0.4 | 引数のキャスト（コンテキストの推測）方法を変更 https://dev.mysql.com/doc/refman/8.0/ja/comparison-operators.html **注記** |
| `InteriorRingN()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `Intersects()` | 5.7.6 (D) 8.0.0 (R) | `MBR` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `IsClosed()`, `IsEmpty()`, `IsSimple()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `JSON_MERGE()` | 5.7.8 8.0.3 (D) | 5.7.8 で導入→非推奨に https://dev.mysql.com/doc/refman/8.0/ja/json-modification-functions.html#function_json-merge `JSON_MERGE_PRESERVE()`に置き換え |
| `LEAST()` | 8.0.4 | 引数のキャスト（コンテキストの推測）方法を変更 https://dev.mysql.com/doc/refman/8.0/ja/comparison-operators.html **注記** |
| `LineFromText()`, `LineStringFromText()`, `LineFromWKB()`, `MultiLineStringFromWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `LineString()` | 5.7.6 8.0.0 ほか | GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `MASTER_POS_WAIT()` | 8.0.26 (D) | 非推奨→`SOURCE_POS_WAIT`へ置き換え（Aurora MySQL v3 バックポート済み） |
| `MATCH()` | 8.0.28 (NR) | ロールアップカラム使用不可に https://dev.mysql.com/doc/refman/8.0/en/fulltext-search.html#function_match |
| `MBREqual()` | 5.7.6 (D) 8.0.0 (R) | `MBREquals()` に置き換え GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| その他`MBR`で始まる関数 | 5.7.6 8.0.0 ほか | GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `MLineFromText()`, `MultiLineStringFromText()`, `MLineFromWKB()`, `LineStringFromWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `MPointFromText()`, `MultiPointFromText()`, `MPointFromWKB()`, `MultiPointFromWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `MPolyFromText()`, `MultiPolygonFromText()`, `MPolyFromWKB()`, `MultiPolygonFromWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `MultiLineString()`, `MultiPoint()`, `MultiPolygon()` | 5.7.6 8.0.0 ほか | GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `NumGeometries()`, `NumInteriorRings()`, `NumPoints()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `OLD_PASSWORD()` | 5.7.5 (R) | 5.6 時点で非推奨→廃止 |
| `Overlaps()` | 5.7.6 (D) 8.0.0 (R) | `MBR` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `PASSWORD()` | 5.7.6 (D) 8.0.11 (R) | `CREATE USER`や`GRANT`（こちらは暗黙のユーザ作成機能自体も廃止）で`IDENTIFIED BY PASSWORD()`が不可に https://dev.mysql.com/doc/refman/5.7/en/encryption-functions.html#function_password |
| `Point()`, `Polygon()` | 5.7.6 8.0.0 ほか | GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `PointFromText()`, `PointFromWKB()`, `PointN()`, `PolyFromText()`, `PolygonFromText()`, `PolyFromWKB()`, `Polygon()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `PROCEDURE ANALYSE()` | 5.7.18 (D) 8.0.0 (R) | 8.0.0 で削除 https://dev.mysql.com/doc/refman/5.7/en/procedure-analyse.html |
| `ROUND()`, `TRUNCATE()` | 8.0.21 | 戻り値の型を決める方法を変更 https://dev.mysql.com/doc/refman/8.0/ja/mathematical-functions.html#function_round |
| `SRID()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `ST_`で始まる関数 | 5.7.6 8.0.0 ほか | GIS 関数刷新の影響を受ける可能性がある **[(\*2)](#2)** |
| `StartPoint()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `SUBTIME()` | 8.0.28 (NR) | 戻り値の型を決める方法を変更 https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_addtime |
| `Touches()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `UNIX_TIMESTAMP()` | 8.0.28 (NR) | 64 ビット環境では最大値が `32536771199.999999`（`'3001-01-18 23:59:59.999999' UTC`） に https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_unix-timestamp |
| `UpdateXML()` |  | XML 関数は開発中の機能なので随時修正が入る https://dev.mysql.com/doc/refman/8.0/ja/xml-functions.html |
| `VALUES()` | 8.0.20 (D) | `INSERT ... ON DUPLICATE KEY UPDATE`で`UPDATE`句の`VALUES()`が非推奨に https://dev.mysql.com/doc/refman/8.0/ja/miscellaneous-functions.html#function_values |
| `WAIT_UNTIL_SQL_THREAD_AFTER_GTIDS` | 8.0.18 (D) | 非推奨に→`WAIT_FOR_EXECUTED_GTID_SET()`で代替 https://dev.mysql.com/doc/refman/8.0/ja/gtid-functions.html#function_wait-until-sql-thread-after-gtids |
| `Within()` | 5.7.6 (D) 8.0.0 (R) | `MBR` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `X()`, `Y()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| （型）`YEAR()` | 5.7.5 (R) | 関数ではないが、`YEAR(2)`型が廃止 https://dev.mysql.com/doc/refman/5.7/en/migrating-from-year2.html |

---

### (\*1)

詳しくはこちら。

- https://dev.mysql.com/doc/refman/8.0/ja/bit-functions.html
  - MySQL 8.0 と 5.7 以前の違い（64 ビットを超えるビット演算に対応したことによる非互換）
- https://dev.mysql.com/doc/refman/5.7/en/bit-functions.html
  - 非互換が生じるオペレータ・関数と変更方法など

### (\*2)

GIS 機能の実装が MySQL 5.6 → 5.7 → 8.0 ですべて変わっているので、使用している場合は実際の動作確認が必要。

- https://dev.mysql.com/doc/refman/5.6/ja/spatial-analysis-functions.html
- https://dev.mysql.com/doc/refman/5.7/en/spatial-analysis-functions.html
  - https://www.slideshare.net/yoyamasaki/mysql-57gisfoss4g-tokai
- https://dev.mysql.com/doc/refman/8.0/ja/spatial-analysis-functions.html
  - https://www.slideshare.net/yoyamasaki/mysql-80gisfoss4g-2018-hokkaido
