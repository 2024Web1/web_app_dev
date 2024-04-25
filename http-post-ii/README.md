# POSTメソッド②

- [POSTメソッド②](#postメソッド)
  - [事前準備](#事前準備)
  - [さまざまな入力フォーム](#さまざまな入力フォーム)
    - [ラジオボタン　・・　選択できるのは１つだけ](#ラジオボタン選択できるのは１つだけ)
    - [チェックボックス　・・　複数選択できる](#チェックボックス複数選択できる)
  - [隠しフィールド（ hidden : ヒドゥン ）](#隠しフィールド-hidden--ヒドゥン-)
  - [付録: hiddenの詳細](#付録-hiddenの詳細)

## 事前準備

前回の`06_POSTメソッド1`でcloneしたコードをそのまま利用する。

```text
C:¥xampp¥htdocs
    └── 06_post-GitHubのユーザー名
        └── src
            └── form
                ├── checkbox.html
                ├── checkbox.php
                ├── hidden.html
                ├── hidden.php
                ├── <中略>
                ├── radio.html
                └── radio.php

```
<div style="page-break-before:always"></div>

## さまざまな入力フォーム

### ラジオボタン　・・　選択できるのは１つだけ


**radio.html** 

![](./images/06/radio_html_display.png)</br>
※画面を開いた時点で「リンゴ」にチェックが入っている

**radio.php**
![](./images/06/radio_php_display.png)

<div style="page-break-before:always"></div>

**radio.html**

![](./images/06/radio_html_code.png)

`<input>`タグに、`checked`を設定すると、画面を開いたときにそのラジオボタンが選択された状態となる。

<div style="page-break-before:always"></div>

**radio.php**

![](./images/06/radio_php_code.png)


### チェックボックス　・・　複数選択できる

**checkbox.html**

![](./images/06/checkbox_html_display.png)

＊上の図は「オレンジ」と「メロン」を選択している

<div style="page-break-before:always"></div>

**checkbox.php**

![](./images/06/checkbox_php_display.png)

**checkbox.html** 

![](./images/06/checkbox_html_code.png)

<div style="page-break-before:always"></div>

**checkbox.php**

![](./images/06/checkbox_php_code.png)

`$fruit` の後ろに  `.`(ドット)があり、その後ろの `' '` は半角スペースをシングルクォーテーションで囲んでいるので、入力に注意すること。（くだものの名前と名前の間に空白を入れるため）

<div style="page-break-before:always"></div>

## 隠しフィールド（ hidden : ヒドゥン ）

**hidden.html** 

![](./images/06/hidden_html_display.png)

**hidden.php** 

※パラメータ=data、値=PHP でデータが送られてくるので、`$_POST['data']`で値を受け取ることができる。</br>

![](./images/06/hidden_php_display.png)

<div style="page-break-before:always"></div>

**hidden.html** 

![](./images/06/hidden_html_code.png)

**hidden.php**

![](./images/06/hidden_php_code.png)

<div style="page-break-before:always"></div>

## 付録: hiddenの詳細

フォームの送信時に、ブラウザに非表示のデータをサーバーに送信することができる。ユーザーには表示されないため、UIの見栄えを損ねずに必要なデータを送信できる。

※作成した`hidden.html`をブラウザに表示し、右クリック→「ソースの表示」でソースコードを確認すると、送信データ(パラメータ=data、値=PHP)が確認できる。</br>

![](./images/06/hidden_data.png)

**つまり、ソースコードで見えるということは、本当に大事な情報は扱えないということ。**
