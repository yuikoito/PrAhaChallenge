## 課題１

- もし子が更に子を持つ構成になった場合に、無限にツリー構造が発生することになる
- parent_message_id 自体の正規化ができないので、null or string という形式になってしまう
- 親と子でテーブル構造が今後変わることがある場合、テーブルを分けるのが困難

ただし、逆にスラックのように孫が今後絶対に発生しないという条件なのであれば、この構成は許容できるかなという気もする・・

## 課題２

![image](https://raw.githubusercontent.com/yuikoito/PrAhaChallenge/master/db/anti-patern-4/Diagram.drawio.png)

閉包テーブルで対応

## 課題３

Notion や Jira のようなアプリ
要は親子の構成が似ているまたは同じであり、親子関係が生じる場合に発生する
