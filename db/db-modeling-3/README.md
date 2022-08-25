## 課題 1

![image](https://raw.githubusercontent.com/yuikoito/PrAhaChallenge/master/db/db-modeling-3/DB3%20Diagram%20task1.drawio.png)

### 親ディレクトリと小ディレクトリの関係性を表すテーブル（ディレクトリ間中間テーブル）を作らなかった理由

- 親ディレクトリの中に、子ディレクトリと同時にドキュメントも入る可能性がある
- そのため、もし作るのであれば、ドキュメントも子になりうることを考慮する必要がある
- そうするとテーブルとしては、親 ID、子 ID として持ちたいのに、親 ID、子ディレクトリ ID、子ドキュメント ID の３つをもつ事になり、完全な親子テーブルとは言いにくい。もし関係性を表すためだけの子 ID をドキュメント、ディレクトリそれぞれに発行して使うのなら可能だが、その場合発行された子 ID と自身の ID が衝突しないようにする必要があり、整合性を保つために労力がかかる。
- また、ドキュメントとディレクトリの構成が似ているため、インターフェースを似せておくことで、同じ検索クエリを使うことが出来て楽。もしディレクトリだけに中間テーブルを用意すると検索方法が変わるので、実装にも時間がかかる。

## 課題 2

![image](https://raw.githubusercontent.com/yuikoito/PrAhaChallenge/master/db/db-modeling-3/DB3%20Diagram%20task2.drawio.png)
