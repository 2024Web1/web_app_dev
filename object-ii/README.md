# オブジェクト指向プログラミング2

- [オブジェクト指向プログラミング2](#オブジェクト指向プログラミング2)
  - [事前準備](#事前準備)
    - [obj\_select.php](#obj_selectphp)
    - [obj\_update.php](#obj_updatephp)
    - [obj\_insert.php](#obj_insertphp)
    - [obj\_delete.php](#obj_deletephp)
    - [obj\_select1.php](#obj_select1php)
  - [【まとめ】オブジェクト指向プログラミングのメリット](#まとめオブジェクト指向プログラミングのメリット)
  - [付録: MVCモデル](#付録-mvcモデル)
  - [課題の作成と提出](#課題の作成と提出)
    - [作成したソースコードをpush](#作成したソースコードをpush)
  - [採点について](#採点について)
    - [課題の合格基準](#課題の合格基準)
    - [合格確認方法](#合格確認方法)
    - [エラーが出た時の対処法](#エラーが出た時の対処法)
    - [タイムアウトになっていないかを確認する](#タイムアウトになっていないかを確認する)
    - [プログラムが正確に書かれているか確認する](#プログラムが正確に書かれているか確認する)
      - [どこでエラーがでているか確認する](#どこでエラーがでているか確認する)
      - [プログラムが正確に書かれているか確認する](#プログラムが正確に書かれているか確認する-1)
  - [GitHub上での採点についてのお願い](#github上での採点についてのお願い)

## 事前準備

前回の「11.オブジェクト指向プログラミング1」でcloneしたコードをそのまま利用する。

```text
C:¥xampp¥htdocs
    └── 11_obj-GitHubのユーザー名
        ├── <中略>
        └── src
        |   └── classes
        |   |   ├── dbdata.php
        |   |   └── dbphp.php
        |   ├── obj_delete.php
        |   ├── obj_insert.php
        |   ├── obj_select.php
        |   ├── obj_select1.php
        |   └── obj_update.php
        |   └── script
        |   |   └── mysql_php.txt
        └── <中略>
```

前章の「11_オブジェクト指向プログラミング1」でクラス「DbPhp」を利用するphpファイルのコードを示す。

<div style="page-break-before:always"></div>

### obj_select.php

![](./images/11/obj_select_code.png)

①： `require_once __DIR__ . '/classes/dbphp.php';`</br>
このコードでDbPhp.class が利用できるようになる

②: `$dbPhp = new DbPhp( );`</br>
DbPhpクラスのオブジェクトを生成し、変数 `$dbPhp` に格納する。

③: `$persons = $dbPhp->selectAll( );`</br>
DbPhpオブジェクトの `selectAll( )` メソッドを呼び出し、抽出したデータが格納されているPDOオブジェクトを受け取るステートメント

④: `foreach ($persons as $person) {`</br>
受け取ったPDOステートメントオブジェクトから1件ずつデータを取り出している 

<img src="./images/11/obj_select_display.png" width="75%">

<div style="page-break-before:always"></div>

### obj_update.php

![](./images/11/obj_update_code.png)
![](./images/11/obj_update_display.png)

<div style="page-break-before:always"></div>

### obj_insert.php

![](./images/11/obj_insert_code.png)
![](./images/11/obj_insert_display.png)

<div style="page-break-before:always"></div>

### obj_delete.php

![](./images/11/obj_delete_code.png)
![](./images/11/obj_delete_display.png)

<div style="page-break-before:always"></div>

### obj_select1.php

![](./images/11/obj_select1_code.png)

1. uid=2が指定されたとき</br>
クエリパラメータを設定した以下のURLにブラウザからアクセスする。</br>
`http://localhost/11_obj-GitHubのユーザー名/src/obj_select1.php?uid=2`</br>
![](./images/11/obj_select1_display1.png)

<div style="page-break-before:always"></div>

2. uidの指定がないとき</br>
以下のURLにブラウザからアクセスする。</br>
`http://localhost/11_obj-GitHubのユーザー名/src/obj_select1.php`</br>
![](./images/11/obj_select1_display2.png)

3. データのないuid=9が指定されたとき</br>
クエリパラメータを設定した以下のURLにブラウザからアクセスする。</br>
`http://localhost/11_obj-GitHubのユーザー名/src/obj_select1.php?uid=9`</br>
![](./images/11/obj_select1_display3.png)

## 【まとめ】オブジェクト指向プログラミングのメリット

オブジェクト指向プログラミングのメリットは、**複雑なロジック部分のコードを分離することができる** ということである。

さらに、PHPのコードを排除した画面用のコード作成することも可能となる。こうして、プログラマーとデザイナーの役割に応じて開発を同時に進めていくことができるようになる。

現状では、こうしたWebアプリケーションの開発で「**MVCモデル**」というデザインパターンがよく利用されており、「CakePHP」や「Laravel」といったフレームワークにもこの「MVCモデル」の概念が取り入られている。（「Ruby on Rails」なども同様）

## 付録: MVCモデル

MVCとはModel・View・Controllerの略で、処理を３つの役割に分割して実装する手法。</br>

![](./images/11/Aspose.Words.a4c93f43-ec41-42b5-b372-9be25bdbba96.013.jpeg)

- Controller: クライアントからのリクエストを直接受け取り処理を行う部分で、ModelやViewを「制御」する。
- Model: 処理のメインロジックやデータアクセスを担当する。
- View: 処理結果として画面表示（HTML出力）を担当する。

処理の流れとしては、以下のようになる。Controllerが最も前面かつ全ての仲介に位置する。

1. Controllerがリクエスト情報を基にModelに処理を依頼
1. Modelはデータと連携して処理を行い、処理結果をControllerに返す
1. Controllerは返ってきた処理結果データをViewに渡す
1. Viewはデータを基にHTML出力処理を行う

## 課題の作成と提出

### 作成したソースコードをpush

pushまでの説明は省略する。忘れた場合は、これより以前の資料を見返し確認すること。

## 採点について

提出した課題はGitHub上で自動採点される。提出後、課題が合格しているかを確認すること。合格していない場合は修正後pushし、再提出すること。

### 課題の合格基準

以下の3つを合格基準とする。

1. obj_update.phpにて、データが正しく更新されること
1. obj_insert.phpにて、データが正しく挿入されること
1. obj_delete.phpにて、データが正しく削除されること

<div style="page-break-before:always"></div>

### 合格確認方法

1. 本課題の[課題ページ](https://classroom.github.com/a/cZKEEdEs)に再度アクセスする。
2. 画面上部にある`Actions`をクリックする。</br>
![](./images/11/acions.png)
1. **一番上**の行に、緑色のチェックが入っていればOK。※その下に赤いばつ印が入っているものがあるが、それは無視する。</br>
![](./images/11/pass.png)

### エラーが出た時の対処法

自動採点がエラーになると、**一番上**の行に赤いばつ印がでる。その場合の解決策を以下に示す。

### タイムアウトになっていないかを確認する

※右端の赤枠で囲まれている箇所に処理時間がでるが、**2分以上**かかっている場合はタイムアウトとなる。
![](./images/11/timeout.png)

なお、タイムアウトの場合は、GitHub上で処理を再開すると解決できる。具体的なタイムアウト解決方法は、

  1. Actionsタブをクリック
  2. タイトルが下記のようにリンクになっているので、クリック</br>
      ![](./images/11/timeout2.png)</br>
  3. Autogradingをクリック</br>
   <img src="./images/11/timeout3.png" width="75%"></br>
   <div style="page-break-before:always"></div>

  4. 赤いばつ印が出ている箇所をクリック</br>
   <img src="./images/11/timeout4.png" width="75%"></br>
  1. `::error::Setup timed out in 120000 milliseconds`のメッセージがあればタイムアウト

  6. 右上に`Re-run jobs`(再実行)のボタンがあるので、`Re-run failed jobs`(失敗した処理だけ再実行)をクリックする。
  ![](./images/11/timeout6.png)</br>
  ![](./images/11/timeout7.png)
  7. タイムアウトにならず2分以内に処理が終了したらOK。※タイムアウトでないエラーは、次の解決策を参照。

<div style="page-break-before:always"></div>

### プログラムが正確に書かれているか確認する

プログラムが正確に書かれているかを確認すること。たとえ、ブラウザの画面でそれっぽく表示されても、自動採点なので融通がきかない。エラーが出た際は、以下を確認すること。

#### どこでエラーがでているか確認する

今回は3つの自動採点(update, insert, delete)があるので、以下の手順で、どごでエラーが出ているか確認する。

1. Actionsタブをクリック
2. タイトルが下記のようにリンクになっているので、クリック
      ![](./images/11/timeout2.png)</br>
3. Autogradingをクリック</br>
   <img src="./images/11/timeout3.png" width="65%"></br>
4. 赤いばつ印が出ている箇所をクリック</br>
  <img src="./images/11/timeout4.png" width="65%"></br>
1. エラーがあるソースコードは、下記画像のように、エラーメッセージが表示されるので、これにエラーが出ているソースコードを特定できる。</br>
<img src="./images/11/error_message.png" width="75%"></br>

#### プログラムが正確に書かれているか確認する

プログラムが正確に書かれているかを確認する。たとえ、ブラウザの画面でそれっぽく表示されても、自動採点ですので融通はききません。エラーが出た際は、サンプルコードと差異がないか確認してください。

<div style="page-break-before:always"></div>

## GitHub上での採点についてのお願い

今回、再度GitHub上での採点をするにあたりお願いがあります。それは、</br>
GitHubに課題をpushする前に、**必ずブラウザで動作確認をしてください。**　理由は下記の2つです。</br>

1. Webアプリケーションはブラウザ上で動作することが前提であるため。
2. GitHubの採点処理時間に上限があるため。</br>
以前の自動採点プログラムと比べ、処理時間の高速化には成功したものの、GitHubの合計処理時間には毎月上限があります。むやみやたらにpushすると、上限に達しかねないので、必ずブラウザ上で正常に動作することを確認してからpushしてください。**エラーの原因が特定できない場合は、お気軽に質問してください。**

