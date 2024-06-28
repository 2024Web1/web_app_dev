# ログイン認証③

- [ログイン認証③](#ログイン認証)
  - [事前準備](#事前準備)
  - [logout.php](#logoutphp)
  - [これでログイン認証作成完了！！...ではありません](#これでログイン認証作成完了ではありません)
    - [login\_check.php](#login_checkphp)
    - [welcome.php](#welcomephp)
  - [課題の作成と提出](#課題の作成と提出)
  - [採点について](#採点について)
    - [課題の合格基準](#課題の合格基準)
    - [合格確認方法](#合格確認方法)

## 事前準備

[ログイン認証①](../login-i/README.md)でcloneしたコードをそのまま利用してください。

ログイン認証②まで作成していれば、以下のファイル構成となっているはずです。

```text
public
├── classes
│   ├── dbdata.php
│   └── user.php
├── index.php
├── login_check.php
├── login.html
├── register.html
├── register.php
├── util.php
└── welcome.php
```

それでは、引き続きログイン認証のプログラムを実装していきましょう！

## logout.php

ログアウト処理を行う`logout.php`を作成します。

なお、**一部穴埋めになっている**ので、それぞれの箇所に適切なコードを追記してください

```php
<?php
// セッションを開始(穴埋め)

// $_SESSIONの中身を空にする
$_SESSION = array();
// クッキーの中にセッションIDがある場合、クッキーの中身を空にする
// session_name()は、セッションIDが入っているクッキーの名前を取得する関数
if (isset($_COOKIE[session_name()]) == true) {  // ①
    setcookie(session_name(), '', time() - 10, '/');
}
// セッションに 関連づけられた全てのデータを破棄する(破棄タイミングは、PHPスクリプトが実行された後)
session_destroy();
?>

<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ログアウトページ</title>
    <link rel="stylesheet" href="css/login.css">
</head>

<body>
    <div id="main">
        <h2>ログアウトしました</h2>
        <hr><br>
        <p><a href="login.html">ログインページへ</a></p>
    </div>
</body>
```

## これでログイン認証作成完了！！...ではありません

このままですと、実はログインしていなくても、直接URLを入力することで認証結果画面(welcome.php)にアクセスすることができてしまいます...(もちろん、正しい画面遷移ではないのでエラーが出ます)

![](./images/welcome_php_display_error.png)

このような不正アクセスを防ぐために、ログインしていない場合はログインページに遷移をするよう処理を追加します。

### login_check.php

まずは、認証処理画面(login_check.php)にて、ログインに成功した際、セッションに認証情報を保存します。

```php
<?php
// 送られてきたユーザーIDとパスワードを受け取る(穴埋め)
$userId   = 
$password = 

// Userクラスを利用するため、user.phpクラスを読み込む(穴埋め)
require_once 
// UserクラスからUserオブジェクトを生成する(穴埋め)
$user 
// authUserメソッドを呼び出し、認証結果を受け取る(穴埋め)
$result = 

// ログインに成功した場合、welcome.phpにリダイレクトする
if ($result) { 
    // セッションを開始(穴埋め)
    
    // --ここだけ追加--
    $_SESSION['login'] = 1;　// ログインフラグ
    // --ここまで--
    // セッションにユーザー名を保存
    $_SESSION['userName'] = $result['userName'];
    header('Location: welcome.php');
    exit();
}

// 共通するデータ・関数を定義したPHPファイルを読み込む
require_once  __DIR__  .  '/util.php';
?>

<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ログインページ</title>
    <link rel="stylesheet" href="css/login.css">
</head>

<!-- ログインに失敗した場合のメッセージを表示する -->

<body>
    <div id="main">
        <h2>ログインに失敗しました</h2>
        <hr><br>
        ユーザーID、パスワードを確認してください。
        <p><a href='login.html'>ログインページへ</a></p>
    </div>
</body>

</html>
```

### welcome.php

続いて、認証結果画面(welcome.php)にて、セッションに認証情報が無い場合に、ログイン画面にリダイレクトする処理を追加します。

```php
<?php
// セッションを開始(穴埋め)

// ----ここから追加----
if (isset($_SESSION['login']) == false) {
    header('Location: login.html');
    exit();
}
// ----ここまで追加----

// 共通するデータ・関数を定義したPHPファイルを読み込む
require_once  __DIR__  .  '/util.php';
?>

<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ログインページ</title>
    <link rel="stylesheet" href="css/login.css">
</head>

<body>
    <div id="main">
        <h2>ようこそ！</h2>
        <hr><br>
        <!-- セッションに保存されたユーザー名を表示する(穴埋め) -->
        <?= h(                 ) ?>さんログイン中
        <p><a href='logout.php'>ログアウト</a></p>
    </div>
</body>

</html>
```

以上で、修正は完了です。
実際の動作を確認してみましょう。

1. ログインしていない状態で、welcome.phpにアクセスしてください。<br>
![](./images/url.png)

2. ログインページにリダイレクトされることを確認してください。
![](./images/login_html_display.png)

これで本当にログイン認証の作成が完了しました。
お疲れ様でした。

## 課題の作成と提出

## 採点について

提出した課題はGitHub上で自動採点されます。
提出後、課題が合格しているかを確認してください。
合格していない場合は修正後pushし、再提出してください。

### 課題の合格基準

以下の3つを合格基準とします。

1. ユーザーが新規登録できること
2. 新規登録したユーザーでログインできること
3. ログインしていない状態で、welcome.phpにアクセスすると、ログインページにリダイレクトされること

### 合格確認方法

1. 本課題の[課題ページ](https://classroom.github.com/a/Z_5J2cM9)に再度アクセスします。
2. 画面上部にある`Actions`をクリックしてください。<br>
![](./images/acions.png)
1. **一番上**の行に、緑色のチェックが入っていればOKです。<br>
![](./images/pass.png)
