# Cookie

- [Cookie](#cookie)
  - [事前準備](#事前準備)
  - [Cookieとは](#cookieとは)
  - [Cookieの基本仕様](#cookieの基本仕様)
  - [サンプル](#サンプル)
    - [cookie1](#cookie1)
    - [cookie2](#cookie2)
    - [cookie3](#cookie3)
    - [cookie4](#cookie4)

## 事前準備

[こちらのページ]()から、ソースコードを`C:¥web_app_dev`へcloneしてください。

## Cookieとは

Webサーバーが、Webブラウザを通じてユーザーのコンピュータに一時的にデータを書き込んで保存する仕組みです。Netscape Communications社が同社のブラウザにCookieを組み込んだのが始まりです。(1994年)

ユーザに関する情報や最後にサイトを訪れた日時、そのサイトの訪問回数などを記録し、ユーザーの識別に使われ、認証システムや、Webによるサービスをユーザごとにカスタマイズするパーソナライズシステムの要素技術として利用されています。

本来、サーバサイド技術においては、サーバからクライアント側に対して書き込みを行うことは許されません。<br>
→データを書き込むことは、対象の端末を操作するということだからです。

ちなみに、ChromeやFirefoxのブラウザで「F12」キーをクリックすると「デベロッパーツール」が表示され、そこでCookieの値を確認することができます。

Chromeの場合だと、以下の手順で確認できます。
1. 「F12」キーでデベロッパーツールを開く
2. デベロッパーツールの「アプリケーション」タブを選択する
3. 「ストレージ」のセクションにある「Cookies」アイコンを開く
![](./images/devtool.png)

## Cookieの基本仕様

HTTPレスポンス・メッセージの「Set-Cookie:」ヘッダーで発行するCookie情報を送信します。Webアプリケーション上におけるCookieについての流れは以下のイラストのとおりです。

![](./images/cookie_image.png)<br>

発行するCookieには、「名前(Cookie名)とその値」、「有効期限」「適用範囲（ドメイン名とパス名）」などが記述されています。

- 名前(Cookie名)とその値 **（必須）**<br>
Cookie名は、Webサーバ側で決定し、名前を変えて複数のCookieを発行することができます。
値はCookieそのものを表し、この値でユーザーを識別します。

- 有効期限
  - 固定Cookie
    - 有効期限がセットされている場合、そのCookieをハードディスクに保存します。
  - セッションCookie
    - 有効期限がセットされていない場合、パソコン内のメモリーにだけ保持し、Webブラウザを閉じると同時に消えます。

- 適用範囲<br>
Cookieの適用範囲とは、WebブラウザがCookieを送り返すWebサーバーを指し、ドメイン名とパス名で表します。<br><br>
例えばWebサーバーから、`domain=sample.com; path＝/auth` といった適用範囲のCookieが発行された場合、「sample.com」の「/auth」ディレクトリ以下にアクセスする場合のみCookieを送ります。
同じ「sample.com」のサイトでも、他のディレクトリへアクセスする場合には送りません。<br><br>
Webサーバーのドメイン名と異なるドメイン名を適用範囲とするCookieは無効と判断し、Cookieはブラウザに保存されません。
適用範囲の情報がセットされていなかった場合、受信したWebサーバーのドメイン名とパス名を自動的にセットします。

## サンプル

`public`ディレクトリに、`cookie1.php`、`cookie2.php`、`cookie3.php`、`cookie4.php`を作成してください。

### cookie1

ブラウザ（クライアント）からApacheサーバに `cookie1.php` へのリクエストを送信します。Apacheサーバは `cookie1.php` のHTMLをレスポンスしますが、この時点ではまだCookieは生成されていません。イラストで表すと以下のようになります。<br>
![](./images/cookie_image_12.png)<br><br>
ブラウザは、`cookie1.php` の内容を画面に表示する。（下図はユーザー名「神戸」を入力した状態）<br>
![](./images/cookie1_display.png)<br><br>

`cookie1.php`

```php
<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cookie1</title>
</head>

<body>
  <h3>Cookie1</h3>
  <form method="POST" action="cookie2.php">
    ユーザー名：<input type="text" name="user_name">
    <input type="submit" value="送信">
  </form>
</body>

</html>
```

この画面で、ユーザー名に「神戸」と入力し、送信ボタンを押すと、`cookie2.php`にデータが送信されます。

### cookie2

`cookie2.php` は、受信した「ユーザー名」の値（神戸）をもとにクッキー名`cookie_name` でCookieを生成します。
そして、「神戸さん、ようこそ！」のHTMLとともに、生成したCookieをクライアントにレスポンスします。

ブラウザは、送信されてきたCookieを保存し、`cookie2.php` の内容を画面に表示します。イラストと画面表示は次のようになります。
![](./images/cookie_image_3456.png)<br><br>
![](./images/cookie2_display.png)<br><br>
「クッキーデータを確認する」リンクをクリックすると、保存したCookieデータとともに `cookie3.php` へのリクエストをApacheサーバに送信します。（保存したデータとは、クッキー名：cookie_name、値：神戸の組み合わせ）<br><br>

`cookie2.php`

```php
<?php
if (isset($_POST['user_name'])  &&  $_POST['user_name']  !=  '') { // ① 
  $user_name  =  $_POST['user_name'];　// ②
  setcookie("cookie_name",  $user_name,  time() + 10);　// ③
}
?>

<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cookie2</title>
</head>

<body>
  <h3>Cookie2</h3>
  <?php
  if (isset($user_name)) {
    echo '<p>' . $user_name . 'さん、ようこそ！</p>';
    echo '<a href="cookie3.php">クッキーデータを確認する</a>';
  } else {
    echo '<p>名前が入力されていません。</p>';
    echo '<a href="cookie1.php">cookie1.phpに戻る</a>';
  }
  ?>
</body>

</html>
```

①: `if(isset($_POST['user_name'])  &&  $_POST['user_name']  !=  '') {`<br>
パラメータ名 `user_name` でデータが送られてきていることを確認しています。<br>

- `isset($_POST['user_name'])`
  - PHPの `isset( )` 関数で、 `$_POST['user_name']` の値があれば `True` を返します。
- `$_POST['user_name'] != ''`
  - `$_POST['user_name']` の値が「空文字」でなければ `True` を返します。

②: `$user_name = $_POST['user_name'];`<br>
パラメータ名 `user_name` で送られてきた値を取得しています。

③: `setcookie("cookie_name", $user_name, time( ) + 10);`<br>
送られてきた値をクッキー名 `cookie_name` で保存するクッキーデータを用意します。
このとき、第3引数の `time( ) + 10` でクッキーの有効期限を設定しています。

`time( )` 関数は、PHPで定義されている関数で、現在時刻をUnixエポック(1970年1月1日 00:00:00 GMT)からの 通算秒 として返す関数で、`time( ) + 10` で現在時刻から10秒間だけ有効なクッキーとしています。以下はもう少し複雑な例です。

- 有効期限を現在時刻から3日間とする場合

  ```php
  time( ) + 60 * 60 * 24 * 3
  // ※60(秒) * 60(分) * 24(時間) * 3(日) = 259,200(秒)
  ```

なお、有効期限を指定しない場合は、**クライアント側のブラウザが閉じられると消えてしまう**クッキーとなります。

### cookie3

`cookie3.php` は、Cookieデータを取得したのち、Cookieデータを破棄するデータとともにレスポンスを返します。イラストと画面表示は以下のようになります。

<img src="./images/cookie_image_7891911.png" width="75%"><br>
![](./images/cookie3_display.png)<br><br>

「破棄後のクッキーデータを確認する」リンクをクリックすると、`cookie4.php` へのリクエストをApacheサーバに送信します。このとき、クッキーデータは破棄されているので、送信されません。

`cookie3.php`

```php
<?php
if (isset($_COOKIE['cookie_name'])) { // ①
  $cookie_name = $_COOKIE['cookie_name'];
  setcookie("cookie_name", '', time() - 10);　// ②
}
?>

<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cookie3</title>
</head>

<body>
  <h3>Cookie3</h3>
  <?php
  if (isset($cookie_name)) {
    echo '<p>Cookieに保存されている名前は「' . $cookie_name . '」ですが、ここで破棄します。</p>';
    echo '<a href="cookie4.php">破棄後のクッキーデータを確認する</a>';
  } else {
    echo '<p>Cookieデータが保存されていません。</p>';
    echo '<a href="cookie1.php">cookie1.phpに戻る</a>';
  }
  ?>
</body>

</html>
```

①: `$_COOKIE['cookie_name']`<br>
`$_COOKIE[ ]` は連想配列。（`$_GET[ ]` や `$_POST[ ]` も連想配列）<br>
②: `setcookie("cookie_name", '', time( ) - 10);`<br>
クッキーを破棄するには、有効期限を昔の時間に設定します。<br>
ここでは、現在時刻から10秒前の時間を設定しています。

ちなみに、`cookie2.php` でCookieの有効期限を `time( ) + 10`で10秒間に設定したが、10秒以上経過した後、`cookie2.php` から `cookie3.php` にアクセスすると、Cookieが保存されていないので以下のような画面になります。

![](./images/cookie3_display_ng.png)<br>

### cookie4

クッキーデータは送信されてこないので、`cookie4.php` がクッキーデータを取得しようとすると、そのようなデータがないという注意メッセージが表示されます。

![](./images/cookie_image_121314.png)<br><br>
![](./images/cookie4_display.png)<br>

**エラーメッセージの意味（要約）**

未定義の配列キーである `cookie_name` が `cookie4.php` の X行目(on line X)に書かれています。
※画像のエラーメッセージは、Windowsとは違う環境で確認しているため、リソースへのパスがWindowsとは異なります。

`cookie4.php`

```php
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cookie4</title>
</head>

<body>
    <h3>Cookie4</h3>
    <?php
    echo $_COOKIE['cookie_name'];
    echo '<p>Cookieデータが破棄されているので、$_COOKIE["cookie_name"]の値は取得できません。</p>';
    echo '<a href="cookie1.php">cookie1.phpに戻る</a>';
    ?>
</body>

</html>
```

`echo $_COOKIE['cookie_name'];`<br>
クッキー名`cookie_name`の値を画面に表示しようとしていますが、すでに破棄されているため、値を取得できない旨のメッセージが表示されます。

**cookie1~4のプログラムを作成完了しても、まだGitHubにpushはしないでください。**<br>
次章の「Session」で作成するプログラムを完成させ、pushしてください。
