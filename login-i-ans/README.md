# ログイン認証(解答)

- [ログイン認証(解答)](#ログイン認証解答)
  - [解答資料について](#解答資料について)
  - [ログイン認証①](#ログイン認証)
    - [classes/user.php](#classesuserphp)
    - [register.php](#registerphp)
  - [ログイン認証②](#ログイン認証-1)
    - [classes/user.php](#classesuserphp-1)
    - [login\_check.php](#login_checkphp)
    - [welcome.php](#welcomephp)
  - [ログイン認証③](#ログイン認証-2)
    - [logout.php](#logoutphp)
    - [welcome.php](#welcomephp-1)

## 解答資料について

穴抜き部分の行のみ、解答を記載しています。
コード全てを記載しているわけではないので、解答例を参考にしながら、自分でコードを書いてみてください。

## ログイン認証①

### classes/user.php

```text
// スーパークラスであるDbDataを利用するため、dbdata.phpを読み込む(穴埋め)
require_once __DIR__ . '/dbdata.php';

// DbDataクラスを継承するUserクラスの定義(穴埋め)
class User extends DbData
    

        // userIdを条件とするSELECT文の定義(穴埋め)
        $sql = 'SELECT * FROM users WHERE userId = ?'; 
        // dbdata.phpのqueryメソッドの実行(穴埋め)
        $stmt = $this->query($sql, [$userId]); 
        // 抽出したデータを取り出す(穴埋め)
        $result = $stmt->fetch();


        // 登録しようとしているユーザーIDが未登録の場合
        // ユーザーを登録するINSERT文の定義(穴埋め)
        $sql = 'INSERT into users(userId, password, userName) VALUES (?, ?, ?)'; 
        // dbdata.phpのexecメソッドの実行(穴埋め)
        $result = $this->exec($sql, [$userId, $password, $userName]);   
```

### register.php

```text
<?php
// 送られてきたデータを受けとる(穴埋め)
$userId   = $_POST['userId'];
$password = $_POST['password'];
$userName  = $_POST['userName'];

// Userクラスを利用するため、user.phpクラスを読み込む(穴埋め)
require_once  __DIR__  .  '/classes/user.php';
// Userオブジェクトを生成する(穴埋め)
$user = new User();
// ユーザー登録処理を行うsignUpメソッドを呼び出し、その結果のメッセージを受け取る(穴埋め)
$result = $user->signUp($userId, $password, $userName);
```

## ログイン認証②

### classes/user.php

```text
        // SQL文を定義(穴埋め)
        // ※ヒント:ユーザーIDとパスワードを条件にユーザー情報を取得
        $sql = 'SELECT * FROM users WHERE userId = ? AND password = ?';
        

        // fetchメソッドでデータを取り出す(穴埋め)
        return $stmt->fetch();
```

### login_check.php

```text
// 送られてきたユーザーIDとパスワードを受け取る(穴埋め)
$userId   = $_POST['userId'];
$password = $_POST['password'];

// Userクラスを利用するため、user.phpを読み込む(穴埋め)
require_once __DIR__  .  '/classes/user.php';
// UserクラスからUserオブジェクトを生成する(穴埋め)
$user = new User();
// authUserメソッドを呼び出し、認証結果を受け取る(穴埋め)
$result = $user->authUser($userId, $password);


    // セッションを開始(穴埋め)
    session_start();
```

### welcome.php

```text
// セッションを開始(穴埋め)
session_start();


        <!-- セッションに保存されたユーザー名を表示する(穴埋め) -->
        <?= h($_SESSION['userName']) ?>さんログイン中
```

## ログイン認証③

### logout.php

```text
// セッションを開始(穴埋め)
session_start();
```

### welcome.php

```text
// セッションを開始(穴埋め)
session_start();
```
