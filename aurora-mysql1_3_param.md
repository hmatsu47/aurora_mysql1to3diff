# Aurora MySQL v1（MySQL 5.6.10a）→ v3（MySQL 8.0.23 またはそれ以降）の間に追加・削除・変更（デフォルト値・項目名など）されたパラメータ一覧

## 調査結果

- **[パラメータ差分調査結果 CSV ファイル](aurora-mysql1_3_param_diff.csv)**

## コメント・補足

- `innodb_file_format`・`innodb_large_prefix`が廃止された
  - `max_allowed_packet`のデフォルトが 64M に上がったので、デフォルトのパラメータグループを変更する点が減った
    - 残りはキャラクタセットと照合順序くらいか？
- `tx_isolation`→`transaction-isolation`と、`master`→`source`・`slave`→`replica`の読み替えには気を付ける
  - 基本的にはデフォルトから変える点は少ないので、あまり問題にならないとは思う
- `innodb_strict_mode`のデフォルトが **厳密モード** に変わった点については気をつけたほうが良い
  - `sql-mode`とあわせて指定を考える
- `innodb_autoinc_lock_mode`のデフォルトが変わった件も、`LAST_INSERT_ID()`の使い方に影響を与える可能性があるので要注意
  - 性能が下がる可能性はあるが`1`に戻したほうが良い場合も