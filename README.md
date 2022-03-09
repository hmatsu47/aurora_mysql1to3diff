# Amazon Aurora MySQL v1 → v3 差分調査用情報置き場

こちらは、

- **[Amazon Aurora MySQL v1（5.6 互換）→ v3（8.0 互換）移行を計画する](https://zenn.dev/hmatsu47/articles/aurora-mysql3-001-top)**

の詳細調査状況公開用リポジトリです。

## 調査状況へのリンク

- **[予約語](mysql57_80_reserved.md)**
- **[オペレータ](mysql57_80_func_oper.md#オペレータ)・[ビルトイン関数](mysql57_80_func_oper.md#ビルトイン関数)**
- **[パラメータ](aurora-mysql1_3_param.md)**

## 調査中

- **[その他マニュアル全般](mysql57_80_manual_all.md)**
  - MySQL 8.0 での検索 391 件中 330 件目（24.6 Restrictions and Limitations on Partitioning）まで確認済み

## 調査対象外

- **`INFORMATION_SCHEMA`・パフォーマンススキーマ・`sys`スキーマの各テーブル・ビュー**
  - 大幅に変更されており、利用対象をピンポイントで調べたほうが効率が良いため
    - https://dev.mysql.com/doc/refman/8.0/ja/information-schema.html
    - https://dev.mysql.com/doc/refman/8.0/ja/performance-schema.html
    - https://dev.mysql.com/doc/refman/8.0/ja/sys-schema.html
  - なお、Aurora MySQL 3.01 時点では、パフォーマンスインサイト（パフォーマンススキーマの情報を利用）で取得可能なデータに異常？がある可能性も
    - https://qiita.com/hmatsu47/items/9db2ad8e8f41e44a54b7#%E3%83%91%E3%83%95%E3%82%A9%E3%83%BC%E3%83%9E%E3%83%B3%E3%82%B9%E3%82%A4%E3%83%B3%E3%82%B5%E3%82%A4%E3%83%88%E3%81%8C