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
| `&` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `&&` | 8.0.17 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/logical-operators.html#operator_and |
| `>>` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `<<` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `^` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `\|` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `\|\|` | 8.0.17 (D) | 非推奨に https://dev.mysql.com/doc/refman/8.0/ja/logical-operators.html#operator_or **注記**も参照（`OR`とは別の使い方←非推奨にはならない） |
| `~` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |

## ビルトイン関数

| 関数 | 変更が加わったバージョン | 変更の概要・参考リンク |
| ---- | ---- | ---- |
| `ADDTIME()` | 8.0.28 (NR) | 戻り値の型を決める方法を変更 https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_addtime |
| `Area()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `AsBinary()`, `AsWKB()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `AsText()`, `AsWKT()` | 5.7.6 (D) 8.0.0 (R) | `ST_` の付かない GIS 関数の廃止 GIS 関数自体の刷新も実施 **[(\*2)](#2)** |
| `BIT_AND()` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `BIT_OR()` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `BIT_XOR()` | 8.0.? | 64 ビットを超えるビット演算に対応 **[(\*1)](#1)** |
| `GREATEST()` | 8.0.4 | 引数のキャスト（コンテキストの推測）方法を変更 https://dev.mysql.com/doc/refman/8.0/ja/comparison-operators.html **注記** |
| `LEAST()` | 8.0.4 | 引数のキャスト（コンテキストの推測）方法を変更 https://dev.mysql.com/doc/refman/8.0/ja/comparison-operators.html **注記** |
| `SUBTIME()` | 8.0.28 (NR) | 戻り値の型を決める方法を変更 https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_addtime |

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
