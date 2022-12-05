## 課題 1

- 親子関係に外部キー制約を設けないと、存在しない親 ID を設定することが可能になってしまう。
  - 今回のケースだと`author_id`が本当に Author テーブルの中に存在する ID であるという保証が持てなくなる。
- 外部キー制約を設けると、必ず親テーブルの ID を参照する必要がある
  - 今回のケースだと作者不明の本があった場合に`author_id`を null にできない
  - 例えば[外部キー制約について](https://qiita.com/SLEAZOIDS/items/d6fb9c2d131c3fdd1387)で上げられている例だと、部署と従業員の関係が上げられている。一時的に従業員が無所属になった場合に部署 ID を null にできないのは困る

## 課題 2

### CASCADE

- 親テーブルの行を削除または更新し、子テーブル内の一致する行を自動的に削除または更新する。ON DELETE CASCADE と ON UPDATE CASCADE の両方がサポートされている。2 つのテーブル間で、親テーブルまたは子テーブル内の同じカラムに対して機能する複数の ON UPDATE CASCADE 句を定義してはいけない。

### SET NULL

- 親テーブルの行を削除または更新し、子テーブル内の 1 つまたは複数の外部キーカラムを NULL に設定する。ON DELETE SET NULL 句と ON UPDATE SET NULL 句の両方がサポートされている。SET NULL アクションを指定する場合は、子テーブル内のカラムを NOT NULL として宣言していないことを確認しないといけない。

### RESTRICT

- 親テーブルに対する削除または更新操作を拒否する。RESTRICT (または NO ACTION) を指定することは、ON DELETE または ON UPDATE 句を省略することと同じである。

### NO ACTION

- 標準 SQL のキーワード。MySQL では、RESTRICT と同等。MySQL Server は、参照されるテーブル内に関連する外部キー値が存在する場合、親テーブルに対する削除または更新操作を拒否する。一部のデータベースシステムは遅延チェックを備えており、その場合、NO ACTION は遅延チェックです。MySQL では、外部キー制約はただちにチェックされるため、NO ACTION は RESTRICT と同じである。

### SET DEFAULT

- このアクションは MySQL パーサーによって認識されるが、InnoDB と NDB はどちらも、ON DELETE SET DEFAULT または ON UPDATE SET DEFAULT 句を含むテーブル定義を拒否する。

参考:
https://dev.mysql.com/doc/refman/5.6/ja/create-table-foreign-keys.html

### 従業員管理サービスの例

- CASCADE だと子テーブル内の一致する行を自動的に削除しまうので、ある部署を消した場合、それにひもづく Employee がすべて消えてしまう。

### プロジェクトマネジメントツールの例

- SET NULL だと DELETE したあとに NULL が設定されてしまうので、担当者が削除されると、NULL が設定されてしまい、担当者が任命されていない Issue が存在することになってしまう。

### 参照アクションにどのようなデフォルト値を設定しているか

- TypeORM

  - Delete -> わからなかった・・・
  - Update -> CASCADE ではないらしいので多分 RESTRICT？
  - ルール以外の更新・削除は一切許容したくないのかも
  - https://github.com/typeorm/typeorm/blob/master/docs/relations.md

- Prisma
  - Delete -> NULL 許容の場合 SET NULL、そうでない場合は RESTRICT
  - Update -> CASCADE
  - Delete が Update よりも RESTRICT な分厳格に守られているので、データを誤って絶対に消したくないような印象を受ける。一方で Update は最悪誤っても戻せばいいので CASCADE なのかも？
  - https://www.prisma.io/docs/concepts/components/prisma-schema/relations/referential-actions#referential-action-defaults

### MySQL と PostgreSQL の違い

- MySQL -> RESTRICT と NO ACTION は同じ
- PostgreSQL -> NO ACTION では遅延チェックが有効になる

## 課題 3

- 外部キー制約の条件は？
  - 親テーブルと子テーブルは同じストレージエンジンでなければならない
  - 親テーブルの参照されるキーと子テーブルの外部キーは、それぞれ同様のデータ型でなければならない
  - 親テーブルの参照されるキーと子テーブルの外部キーにはインデックスが必要
- 主キーの条件は？
  - 主キーは、表内のある行を一意に決めることができる特性をもつ。 候補キーには NULL 値が許されるが、主キーでは NULL 値の存在が許されないため、主キー制約は一意性制約に NOT NULL 制約を加えたものといえる。
- 候補キーの条件は？
  - 主キーの候補となるキーが候補キーで、候補キーの条件は、行を一意に識別できることと極小であること(スーパーキーの中で極小のもの)
