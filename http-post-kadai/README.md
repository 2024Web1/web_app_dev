# 入力フォーム課題

- [入力フォーム課題](#入力フォーム課題)
  - [事前準備](#事前準備)
  - [今回の課題について](#今回の課題について)
  - [パスワード](#パスワード)
    - [password.html](#passwordhtml)
    - [password.php](#passwordphp)
  - [プルダウン](#プルダウン)
    - [pulldown.html](#pulldownhtml)
    - [pulldown.php](#pulldownphp)
  - [テキストエリア](#テキストエリア)
    - [textarea.html](#textareahtml)
    - [textarea.php](#textareaphp)
  - [採点について](#採点について)
    - [課題の合格基準について](#課題の合格基準について)
    - [合格確認方法](#合格確認方法)
    - [エラーが出た時の対処法](#エラーが出た時の対処法)
    - [タイムアウトになっていないかを確認する](#タイムアウトになっていないかを確認する)
    - [プログラムが正確に書かれているか確認する](#プログラムが正確に書かれているか確認する)
      - [どの入力フォームでエラーがでているか確認する](#どの入力フォームでエラーがでているか確認する)
  - [(追記)解答例](#追記解答例)

## 事前準備

[こちらのページ](https://classroom.github.com/a/js1MUStN)から、ソースコードを`C:¥web_app_dev`へcloneしてください。

## 今回の課題について

**本課題以降、ソースコードへの`出席番号 氏名`の記入をやめます。**

今回の課題では、**授業中では紹介されていない入力フォームを使って**プログラムを作成します。そのため、**各自で調査し、作成する必要があります。**

また、作成する各ファイルには、それぞれ作成条件が記載されておりますので、そちらには必ず従ってください。**条件を満たしていなければ、GitHub上の自動テストでエラーになります。**

## パスワード

### password.html

- 作成条件
  - フォームの送り先を `password.php` に設定する
  - フォーム要素はそれぞれ改行する
  - 入力フォームとして、以下のものを設定する
    - テキスト「ユーザーID:」として変数`user`に`text`形式
    - テキスト「パスワード:」として変数`pass`に`password`形式
    - 初期値は設定しない
  - 送信ボタンには名前 `submit` を付与する

![](./images/password_html_display.png)

### password.php

- 作成条件
  - フォームとして受け取った`user`,`pass`の値をそれぞれPHPの命令で出力する
    - 以下の1,2の文章は、それぞれ**別々にpタグでくくる**こと
      1. 「入力されたユーザーIDは、XXXX(`user`の値)です。」
      2. 「入力されたパスワードは、XXXX(`pass`の値)です。」

![](./images/password_php_display.png)<br>
※戻るをクリックすると前の画面に戻ります。

## プルダウン

### pulldown.html

- 作成条件
  - フォームの送り先を `pulldown.php` に設定する
  - 入力フォームとして、以下のものを設定する
    - 入力された値は変数名`fruit`に入る
    - 果物(fruit)の候補として、以下の値を挿入すること(順番は気にしないでよい)
      - オレンジ
      - リンゴ
      - メロン
    - 送信ボタンには名前 `submit` を付与する

![](./images/pulldown_html_display.png)
※ここでは、リンゴを選択しています。

### pulldown.php

- 作成条件
    - フォームとして受け取った`fruit`を出力する
    - 出力するテキストは`p`タグでくくる
    - `p`タグ内のテキストは「あなたの好きなフルーツは、XXX(`fruit`の値)ですね。」とする
  
![](./images/pulldown_php_display.png)
※戻るをクリックすると前の画面に戻ります。

## テキストエリア

### textarea.html

- 作成条件
  - フォームの送り先を `textarea.php` に設定する
  - 入力フォームとして、以下のものを設定する
    - 横幅(1桁の文字数)50、縦幅(行数)4に設定する
    - 入力テキストの渡される変数名を `input_text` とする
    - 送信ボタンには名前 `submit` を付与する
    - テキストエリアの初期値は設定しない

![](./images/textarea_html_display.png)

### textarea.php

- 作成条件
  - 以下の1と2は、それぞれ**別々にpタグでくくる**こと
    1. `入力された文章はつぎのとおりです。`という文章
    2. フォームとして受け取った`input_text`の値

![](./images/textarea_php_display.png)
※戻るをクリックすると前の画面に戻ります。

## 採点について

提出した課題がGitHub上で自動採点されます。pushした課題が合格したかはpush後に必ず確認してください。

### 課題の合格基準について

- ３つの入力フォーム(password, pulldown, textarea)について、POSTメソッドでデータ送信がブラウザで確認できること
- 各作成条件に従い、コーディングできていること

### 合格確認方法

1. 本課題の[課題ページ](https://classroom.github.com/a/js1MUStN)に再度アクセスしてください。
2. 画面上部にある`Actions`をクリックしてください。<br>
![](./images/acions.png)
1. **一番上**の行に、緑色のチェックが入っていればOKです。<br>
![](./images/pass.png)

### エラーが出た時の対処法

自動採点がエラーになると、**一番上**の行に赤いばつ印がでます。その場合の解決策を以下に示します。

### タイムアウトになっていないかを確認する

※右端の赤枠で囲まれている箇所に処理時間がでますが、**4分前後**かかっている場合には、まずタイムアウトの可能性を疑ってください。
![](./images/timeout.png)

具体的なタイムアウトの確認・解決方法は、

  1. `Actions`のタイトルが以下のようにリンクになっているので、クリック
      ![](./images/timeout2.png)
  2. `run-autograding-tests.png`をクリック
   ![](./images/run-autograding-tests.png)
  3. 赤いばつ印が出ている箇所をクリック
  ![](./images/timeout4.png)
  1. `::error::Setup timed out in XXXXXX milliseconds`のメッセージがあればタイムアウト
   ![](./images/timeout8.png)
  6. 解決策としては、右上に`Re-run jobs`(再実行)のボタンがあるので、`Re-run failed jobs`(失敗した処理だけ再実行)をクリックする。
  ![](./images/timeout6.png)<br>
  ![](./images/timeout7.png)
  7. タイムアウトにならず2分以内に処理が終了したらOK。※タイムアウトでないエラーは、次の解決策を参照。

### プログラムが正確に書かれているか確認する

プログラムが正確に書かれているかを確認してください。たとえ、ブラウザの画面でそれらしく表示されても、自動採点なので融通は効きません。エラーが出た際は、以下の点を確認してください。

#### どの入力フォームでエラーがでているか確認する

今回は3つの入力フォーム(password, pulldown, textarea)それぞれのテストがあるので、以下の手順で、どの入力フォームがエラーか確認してください。
※ただし、GitHub上でのエラーメッセージでは、原因の特定が難しい場合もあるので、その場合は適宜質問してください。

1. `Actions`のタイトルが以下のようにリンクになっているので、クリック
  　![](./images/timeout2.png)
2. `run-autograding-tests.png`をクリック
   ![](./images/run-autograding-tests.png)
3. 以下のような3種類のテストの存在が確認できる
   - `TestPassword.py`: passwordに関するテスト
   - `TestPulldown.py`: pulldownに関するテスト
   - `TestTextarea.py`: textareaに関するテスト
   ![](./images/3tests.png)
4. 各テストエラーがあった場合、それぞれのテストでFAILEDと表示される。以下は、`TestPassword.py`(passwordに関するテスト)でエラーが出ている場合の表示例
   ![](./images/testpassword.png)

## (追記)解答例

`password.html`

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>パスワードの練習課題</title>
</head>

<body>
    <!--
    * フォームの送り先を `password.php` に設定する
    * 入力フォームとして、以下のものを設定する
        * テキスト「ユーザーID:」として変数`user`に`text`形式
        * テキスト「パスワード:」として変数`pass`に`password`形式
    * フォーム要素はそれぞれ改行すること
    -->
    <h4>パスワード練習課題</h4>
    <!-- フォームコードは以下に記述-->
    <form method="POST" action="password.php"> <!-- action属性のファイル名を修正-->

        ユーザーID：<input type="text" name="user"><br>
        パスワード：<input type="password" name="pass"><br>

        <input type="submit" name="submit" value="送信">
    </form>
    <!-- ここまで -->
</body>

</html>

```

`password.php`

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>パスワードの練習(送信後)</title>
</head>

<body>

    <!--
    * フォームとして受け取った`user`,`pass`の値をそれぞれPHPの命令で出力する
        * それぞれを**pタグでくくる**こと
        * 「入力されたユーザーIDは、XXXX(`user`の値)です」
        * 「入力されたパスワードは、XXXX(`pass`の値)です」
-->


    <?php
    // PHPのコードは以下に記述
    echo '<p>入力されたユーザーIDは、' . $_POST['user'] . 'です。</p>';
    echo '<p>入力されたパスワードは、' . $_POST['pass'] . 'です。</p>';
    ?>
    <a href='password.html'>戻る</a>
</body>

</html>
```

`pulldown.html`

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>プルダウンの練習</title>
</head>

<body>

    <!--
* フォームの送り先を `pulldown.php` に設定する
* 入力フォームとして、以下のものを設定する
    * 入力された値は変数名fruitに入る
    * 果物(fruit)の候補として、以下の値を挿入すること(順番は気にしないでよい)
        * オレンジ
        * リンゴ
        * メロン
    * 送信ボタンには名前 `submit` を付与する
-->
    <h4>プルダウン練習課題</h4>

    <!-- フォームコードは以下に記述-->
    あなたの好きなフルーツを選んでください。<br> <!-- この行は課題に記載があれば修正する、なければ削除 -->
    <form method="POST" action="pulldown.php"> <!-- action属性のファイル名を修正-->
        <select name="fruit">
            <option>オレンジ</option>
            <option>リンゴ</option>
            <option>メロン</option>
        </select>
        <input type="submit" name="submit" value="送信">
    </form>
    <!-- ここまで -->
</body>

</html>
```

`pulldown.php`

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>プルダウンメニューの練習(結果)</title>
</head>

<body>
    <!--
        * フォームとして受け取った`frult`を出力する
        * 出力するテキストはpタグでくくる
        * pタグ内のテキストは「あなたの好きなフルーツは、XXX(fruitで受け取ったもの)ですね。」とする
    -->

    <?php
    // PHPのコードは以下に記述
    echo '<p>あなたの好きなフルーツは、' . $_POST['fruit'] . 'ですね。</p>';
    ?>
    <a href='pulldown.html'>戻る</a>
</body>

</html>
```

`textarea.html`

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>テキストエリアの練習問題</title>
</head>

<body>
    <!--
* フォームの送り先を `textarea.php` に設定する
* 入力フォームとして、以下のものを設定する
    * 横幅(1桁の文字数)50、縦幅(行数)4に設定する
    * 入力テキストの渡される変数名を `input_text` とする
    * 送信ボタンには名前 `submit` を付与する
-->
    <h4>テキストエリア練習課題</h4>
    <!-- フォームコードは以下に記述-->
    <form method="POST" action="textarea.php"> <!-- action属性のファイル名を修正-->

        <textarea name="input_text" rows="4" cols="50"></textarea><br>

        <input type="submit" name="submit" value="送信">
    </form>
    <!-- ここまで -->

</body>

</html>
```

`textarea.php`

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>テキストエリア練習問題(結果)</title>
</head>

<body>
    <!--
* フォームとして受け取った`input_text`の値を出力する
    * 出力するテキストはpタグでくくる

-->

    <p>入力された文章は、つぎのとおりです。</p>
    <?php
    // PHPのコードは以下に記述
    echo '<p>' . $_POST['input_text'] . '</p>';
    ?>
    <a href='textarea.html'>戻る</a>

</body>

</html>
```
