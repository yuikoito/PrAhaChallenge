## 課題１

- 最大 3 件しかタグの登録ができないので、今後 4 個目、5 個目のタグを登録できるようにした場合にテーブルを大幅に変更する必要がある
- 正規化が出来ていないので、DB 上で型の制約をつけることができない
- １つの POST が何個のタグを持つかを検索する場合に、tag1Id 〜 tag3Id のすべてを見る必要がある

## 課題２

![image](https://raw.githubusercontent.com/yuikoito/PrAhaChallenge/master/db/anti-patern-2/Diagram.drawio.png)

## 課題 3

- ブログの記事とタグの関係
- 出席管理サービス（クラスが複数の生徒を持つ構成）

※要は、１対多・多対多の関係の際に、多と紐付ける場合に最大紐付け数を DB で決めてしまう（変更する場合は多大な負担が生じるような設計にしてしまう）場合に生じる。