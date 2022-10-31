 ## 課題１

- 退会したあとにもう一度登録、再度退会といった場合の履歴が確認できない
- 例えば何度も退会している人がいることなどに気が付けない
- ステータスは本当に退会したかどうかだけで良いのか疑問
  - ステータスが増えたらここにカラムを増やす？
  - そうすると正規化どころの話ではなくなり、テーブルがどんどん肥大化して秩序がなくなっていく・・

## 課題２

![image](https://raw.githubusercontent.com/yuikoito/PrAhaChallenge/master/db/anti-patern-7/Diagram.drawio.png)

※これはイミュータブルデータモデリングのつもりで書いています。
もし退会以外にステータスが増える場合はテーブルが増える設定。

## 課題３

- オーダーを取り消したあと、「前にオーダーしてたあの件って本当に正しくキャンセルされましたか」といった相談が来た場合に、その人が本当に注文してからキャンセルしたのか、そもそも何も注文してなかったのかがわからない
- 顧客側のアプリ（があるとすれば）履歴を正しく見れない。キャンセルした後は自分のオーダーが急に消えたみたいになり違和感があるかも
- 生徒を物理削除した場合、新規登録キャンペーンがある場合、退会を繰り返せば無限にキャンペーンをうけることができてしまう。
- 退会した後やはり再開したい場合に再度個人情報をもらう必要がある
- 退会した後に過去の履歴について質問が来た場合に答えられない
- 過去、物理削除したことがある例はメッセージに関してなど。「一度削除すると二度と戻せませんが大丈夫ですか？」というようなアラートを出して物理削除をした。確かSlackもそんな仕様だった気がする。（どこかのインタビューで物理削除であることを認めていた。ただし、ユーザーが削除するわけではなく、時間経過で削除する場合は復活できるように単に非表示なだけ。）