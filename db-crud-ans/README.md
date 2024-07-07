# データベース利用(解答)

- [データベース利用(解答)](#データベース利用解答)
  - [解答資料について](#解答資料について)
    - [dbselect1.php](#dbselect1php)

## 解答資料について

チャレンジ問題の条件付きSELECT分の穴抜き部分の行のみ、解答を記載しています。
コード全てを記載しているわけではないので、解答例を参考にしながら、自分でコードを書いてみてください。

### dbselect1.php

```text
    // もしも$_GET['uid']が空なら、uidを求めるフォームを表示(GETメソッド使用)
    if (empty($_GET['uid'])) {
    ?>
        <!-- 検索フォームを以下に記述 -->
        <form action="dbselect1.php" method="get">
            <label for="uid">ユーザIDを入力してください：</label>
            <input type="text" name="uid" id="uid">
            <input type="submit" value="検索">
        </form>
        <!-- ここまで -->
        
        
        // ---データベースに接続するためのアカウント情報を以下の変数に設定
        $user = 'sampleuser';
        $password = 'samplepass';
        $host = 'db';
        $dbName = 'SAMPLE';
        $dsn = 'mysql:host=' . $host . ';dbname=' . $dbName . ';charset=utf8';

        
            // PDOを用いてデータベースに接続する
            $pdo = new PDO($dsn, $user, $password);


            // 接続できなかった場合のエラーメッセージ
            exit('データベースに接続できませんでした：' . $e->getMessage());
        
        // uidをキーにして、GETメソッドで受け取ったuidを代入
        $uid = $_GET['uid'];
        // SQLの定義: personテーブルからuidが一致するレコードを取得する
        $sql = 'SELECT * FROM person WHERE uid = ?';
        $stmt = $pdo->prepare($sql);
        // SQLプレースホルダーに値をバインド
        $stmt->execute([$uid]);
        // 一件だけなのでfetch()を使って結果レコードを取得・・・①
        $row = $stmt->fetch();
        // 結果が空ならば、該当するユーザがいない旨を表示
        if (empty($row)) {
            echo '該当するユーザがいませんでした(uid=' . $uid . ')';
        } else {
            // 結果があれば、uidとnameを表示
            echo 'uid=' . $row['uid'] . ', name=' . $row['name'];
        }
        // データベースを切断する
        $pdo = null;
    }
```
