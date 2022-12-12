## 課題 1

- SELECT NULL = 0;

```
NULL = 0
NULL
```

- NULL = NULL (以下、SELECT 部分を省略)

```
NULL = NULL
NULL
```

- NULL <> NULL

```
NULL <> NULL
NULL
```

- NULL AND TRUE

```
NULL AND TRUE
NULL
```

- NULL AND FALSE

```
NULL AND FALSE
0
```

- NULL OR TRUE

```
NULL OR TRUE
1
```

- NULL IS NULL

```
NULL IS NULL
1
```

- NULL IS NOT NULL

```
NULL IS NOT NULL
0
```

※https://paiza.io/ja/projects/new で確認

## 課題２

- テーブル設計の見直し
  - Assignee と Issue の中間テーブルを作成する
- 果たして NULL がデータベースに存在することは本当に悪いことなのか
  - 正規化の観点でいうと存在しないほうが良いと思う
  - ただし、中間テーブルを増やしすぎるのも良くはないので、ケースバイケースだと思う
  - 例えば、ユーザーテーブルがあって、その中に電話番号やメールアドレスを任意で登録できる場合、NULL は許容しても良いと思う
  - それはなぜか: メールアドレステーブルや電話番号テーブルを作って中間テーブルを作るのは現実的ではないから
  - なぜ現実的ではないか: ユーザーの情報という同じ目的のものに対して、任意登録できる項目で新しいテーブルを作ることはテーブルの責務的に微妙な感じがする
  - まとめると: あまり言語化できていないけど、要はテーブルごとで責務が分かれていて、その責務の中で管理しないといけない任意の要素がある場合は NULL を許容してもいいと思う。そうでない場合は、テーブルを細かく分けて、中間テーブルをはやしたほうが良さそう。

## 課題 3

- 実際の値がその DEFAULT 値だった場合、それが DEFAULT 値なのか正しい値なのか判断がつかないから。
  - 例えば: 商品テーブルがあったとして、商品の金額を登録できるとする。そして商品名などから先に決めるため、金額はあとから入力する設計とする（つまり設計時に金額は Optional 扱い）。その場合、初期値で Int: 0 を設定してしまうと、実際にその商品が例えばプレゼント企画等で 0 円で設定されたのか、あるいは金額の設定漏れなのかが判断がつかなくなってしまう。

## 課題 4

- 文字列長さを返す LENGTH 関数の引数が NULL 値であった場合、その戻り値は何になるか
  - MySQL の場合は SELECT CHAR_LENGTH(NULL);の実行結果です
  - 答え: NULL
- 上記で結果を 0 として扱いたい場合はどうすればいいか
  - 答え: NVL 関数を使う
  - MySQL の場合、IfNull を使えば同じ動きをするらしい --> SELECT IfNull(CHAR_LENGTH(NULL), 0);

## 参考

- https://xtech.nikkei.com/it/article/COLUMN/20060302/231519/
- http://kashi.way-nifty.com/jalan/2014/08/mysqlnvl-8dd7.html
