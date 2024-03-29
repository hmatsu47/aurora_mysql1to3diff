# Amazon Aurora MySQL v1 → v3 差分調査用情報置き場

こちらは、

- **[Amazon Aurora MySQL v1（5.6 互換）→ v3（8.0 互換）移行を計画する](https://zenn.dev/hmatsu47/articles/aurora-mysql3-001-top)**

の詳細調査結果公開用リポジトリです。

## 調査状況へのリンク

- **[予約語](mysql57_80_reserved.md)**
- **[オペレータ](mysql57_80_func_oper.md#オペレータ)・[ビルトイン関数](mysql57_80_func_oper.md#ビルトイン関数)**
- **[パラメータ](aurora-mysql1_3_param.md)**
- **[その他マニュアル全般](mysql57_80_manual_all.md)**

## 調査対象外

- **`INFORMATION_SCHEMA`・パフォーマンススキーマ・`sys`スキーマの各テーブル・ビュー**
  - 大幅に変更されており、利用対象のテーブル等をピンポイントで調べたほうが効率が良いため
    - https://dev.mysql.com/doc/refman/8.0/ja/information-schema.html
    - https://dev.mysql.com/doc/refman/8.0/ja/performance-schema.html
    - https://dev.mysql.com/doc/refman/8.0/ja/sys-schema.html

## その他

- 調査結果にはないが、主キー以外のインデックスがないテーブルからのデータ取得時、`ORDER BY`が指定されないケースで複数行結果が返る場合のソート順が不定に
  - 本来これは「不定」が正しいし、MySQL 5.6 のマニュアルにも「不定」と書いてあった
  - 以前は主キー順でデータ取得できたが、MySQL 8.0 では本当に「不定」になった
- パラメータグループでデフォルトどおり`explicit_defaults_for_timestamp`に`1`（有効）を指定した状態で、
  - MySQL Connector/J のプリペアドステートメントを使って
  - `NOT NULL`かつ`DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`の`TIMESTAMP`・`DATETIME`列に対して
  - `null`で`UPDATE`したときの挙動が、以下のように変化するケースがある
    - Aurora MySQL v1・MySQL Connector/J 5.1 の組み合わせ:`UPDATE`時のタイムスタンプで更新
    - Aurora MySQL v3・MySQL Connector/J 8.0 の組み合わせ:「`NOTNULL`列に`null`で`UPDATE`することはできない」旨のエラーが発生
  - 同様の事象が発生した場合は`null`ではなく`NOW()`などで`UPDATE`する形に SQL 文（プリペアドステートメント）を書き換える
- MySQL Connector/J で`FLOAT(M,D)`・`DOUBLE(M,D)`型の列を`java.math.BigDecimal`値として取り込む際に
  - 5.1 時代は`D`（小数点以下の桁数）を自動的にスケールとして設定する実装になっていた
  - 8.0 では明示的な`setScale(D)`が必要になった
- 日付（日時）と文字列の比較
  - https://rabbitfoot141.hatenablog.com/entry/2020/07/01/215728
