## 課題１

- Type を見るまで、belongs_to_id が Manga, Novel どちらに対してのものなのかがわからない。
  - 親である Manga, Novel から見て、属している Comment を見つけにくい（先に Type で grep した上で belongs_to_id の検索をしないといけない）
- 正規化ができない
  - belongs_to_id だけを見て、それが Manga, Novel のどちらに紐づくかがわからないため、その ID が本当に存在する ID なのかどうかがわからない。

## 課題２

![image](https://raw.githubusercontent.com/yuikoito/PrAhaChallenge/master/db/anti-patern-3/Diagram.drawio.png)

それぞれの中間テーブルで対応。
Comment を Manga 用、Novel 用で分けることも考えたが、Comment の内容自体は共通のため、中間テーブルでの対応が良いかと思った。

## 課題３

複数のカテゴリを管理するレビューサイト
例えば、雑貨・服・菓子などのカテゴリをカテゴリ別に管理しているレビューサイトがあった場合、雑貨テーブル・服テーブル・菓子テーブルとレビューを紐付ける際に、今回のようなアンチパターンに応じる気がする
