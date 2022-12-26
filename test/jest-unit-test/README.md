## 課題 2

- https://github.com/yuikoito/praha-challenge-templates/pull/1

## 課題 3

- そもそも、なぜ元の関数はカバレッジ 100%のテストを書けなかったのでしょうか？

  - asyncSumOfArraySometimesZero と、getFirstNameThrowIfLong の中で使われている`new DatabaseMock()`と`new NameApiService()`の部分が mock できず、こちらで返却値を操作することができないから。

- 依存性の注入とは何でしょうか？どのような問題を解決するために使われるのでしょうか？

  - 依存性の注入というのは、「Dependency Injection(DI)」の直訳です。パッと見で何のことかが非常にわかりづらいですが、やりたいこととしては「依存性があるプログラムは保守・テストがしづらいので、それを解決する」ということです。 （https://www.w2solution.co.jp/tech/2021/10/06/eg_ns_rs_izonseinotyunyu/ より）
  - 要は依存性があるものを外部からのインポートにすることにより疎結合にすること

- 依存性の注入を実施することで、モジュール同士の結合度の強さはどのように変化したでしょうか？

  - 弱くなった

- 今回のような単体テストで外部サービスとの通信が発生すると、どのようなデメリットがあるでしょうか？

  - 外部サービスがサービス終了したり、障害が発生した場合に Unit Test では気が付けない。
  - Unit Test をするためには API を mock する必要があるが、mock が間違えていた場合に気が付けない。

- sumOfArray に空の配列を渡すと例外が発生します。現状、あまり好ましい挙動ではありません。「こうなるべきだ」とご自身が考える形に、コードを修正してみてください。
  - 空配列を渡してよいのかどうかの定義による
    - もし空配列を渡して良い場合は、ヒントで書いている場合 reduce の初期値に 0 を設定すると良い。
    - もしそもそも空配列を渡すべきではない設計なら、現状の通り error を吐くでも良いのでは。

```
export const sumOfArray = (numbers: number[]): number => {
  return numbers.reduce((a: number, b: number): number => a + b, 0);
};
```

- コードを修正したら、先ほど書いた単体テストが落ちるはずです

```
describe("test for sumOfArray", () => {
  test("put [] in an argument and should return 0", () => {
    expect(() => sumOfArray([])).toBe(0);
  });
});
```

- Property Based Testing
  - 短いコードで膨大なテストケースを検証できる点、テストケースが開発者に依存せず意図しないエッジケースをテストできる。
- Example Based Testing

  - 入力値に応じて正しい値を返却するかを検証できるので、独自 API を使ったテストなどに向いている？

- 単体テストケースを増やしても可読性、保守性、実行速度などに問題が起きないよう工夫できることを 3 つ考えてみましょう
  - コメントや変数名は誰が見てもわかりやすいような命名を意識する
    - 表面的に見てこれが何をテストするためのコードかが一発でわかるようにする
  - ロジックは単純化する
    - ループを読みやすく簡潔にする
    - スコープを最小限にする
  - テストケースは簡潔なものにする

## 課題 4

- 引数を受け取り、何らかの値を返却する関数を 3 つ

```
function getRandomNumber(min: number, max: number): number {
  return Math.floor(Math.random() * (max + 1 - min)) + min;
}
```

```
function concatenateStrings(str1: string, str2: string): string {
  return str1 + str2;
}
```

```
function isNumberEven(num: number): boolean {
  return num % 2 === 0;
}
```

- jest に関するクイズ

```
次のうち、Jestでテストを開始する方法でないものはどれですか？
A) it()
B) test()
C) describe()
D) assert()

テストにおける非同期コードの処理に関して、Jestのデフォルトの動作はどのようなものですか？
A) Jest は、非同期コードが終了するのを待ってから残りのテストを実行します。
B) Jest は、非同期コードが終了する前にテストの残りの部分を実行します。
C) Jest は非同期コードを別のスレッドで実行します。
D) Jest は非同期コードを別プロセスで実行します。
```

## 課題 5

- https://github.com/electron/forge/blob/main/tools/test-globber.ts
- https://github.com/electron/forge/blob/main/tools/utils.ts

### 学んだこと

- 細かくファイルわけがされており、諸々のテストを test-globber.ts でまとめて管理している。そのため`"test": "xvfb-maybe cross-env LINK_FORGE_DEPENDENCIES_ON_INIT=1 TS_NODE_PROJECT='./tsconfig.test.json' TS_NODE_FILES=1 mocha './tools/test-globber.ts'",`と書くだけでテストが実行できるし、test-globber.ts の中身は非常にわかりやすい。

- 変数 packages は、モジュール内で定義された関数を使用して取得されるパッケージ情報の配列なので、それをまわすという使い方をするだけで各テストが実行できる。それを取得するための抽象レイヤーとして utils.ts が存在している

- for ループを使用して、変数 testFiles 内のすべてのファイルを require() 関数で読み込むことができる
