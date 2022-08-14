## 課題1
### 任意課題

物理モデル...実際のDBで使うキー名で記載された図
論理モデル...人がわかりやすいような名前で記載された図

例：
ユーザーテーブルを物理モデルで書くとすると、以下
```
id: string
name: string
display_name: string
created_at: string
updated_at: string
...
```
論理モデルの場合は以下のようになる

```
id
本名
表示名
作成日
更新日
...
```

## 課題2
- 注文詳細テーブルにシャリの大小のカラムを追加する
- 寿司ネタが毎月何個売れているかは以下の手順で求められるので変更は不要
  1. 注文概要テーブルから注文日で各月分を検索
  2. 注文詳細テーブルから1で求めた注文概要IDに一致する履歴を検索
  3. 2より各メニューIDと個数を求める
  4. 3で求めたメニューIDから、メニューテーブルを検索してメニューの名前（寿司ネタ名）を取得

## 課題3
- 今後は出前を受け付けるようになりました。どのようにテーブル設計をするべきでしょうか?