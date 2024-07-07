# オブジェクト指向プログラミング②(解答)

- [オブジェクト指向プログラミング②(解答)](#オブジェクト指向プログラミング解答)
  - [解答資料について](#解答資料について)
  - [obj\_update.php](#obj_updatephp)
  - [obj\_insert.php](#obj_insertphp)
  - [obj\_delete.php](#obj_deletephp)
  - [条件付きSELECT文にチャレンジ(object\_select1.php)](#条件付きselect文にチャレンジobject_select1php)

## 解答資料について

穴抜き部分の行について、解答を記載しています。
コード全てを記載しているわけではないので、注意してください。

## obj_update.php

`obj_update.php`

```text
// DbPhpクラスのオブジェクト生成し、updatePersonメソッドをよびだす
require_once __DIR__ . '/classes/dbphp.php';
$dbPhp = new DbPhp();
$dbPhp->updatePerson(5, '野口五郎');

// 登録後の全データを画面表示する
$persons = $dbPhp->selectAll();
foreach ($persons as $person) {
    echo 'uid=' . $person['uid'] . ', name=' . $person['name'] . '<br>';
}
```

## obj_insert.php

```text
// DbPhpクラスのオブジェクト生成し、insertPersonメソッドをよびだす
// name = 深沢七郎, company_id = 3, age = 29
require_once __DIR__ . '/classes/dbphp.php';
$dbPhp = new DbPhp();
$dbPhp->insertPerson('深沢七郎', 3, 29);

// 登録後の全データを画面表示する
$persons = $dbPhp->selectAll();
foreach ($persons as $person) {
    echo 'uid=' . $person['uid'] . ', name=' . $person['name'] . '<br>';
}
```

## obj_delete.php

```text
// DbPhpクラスのオブジェクト生成し、deletePersonメソッドをよびだす
require_once __DIR__ . '/classes/dbphp.php';
$dbPhp = new DbPhp();
$dbPhp->deletePerson('深沢七郎');

// 登録後の全データを画面表示する
$persons = $dbPhp->selectAll();
foreach ($persons as $person) {
    echo 'uid=' . $person['uid'] . ', name=' . $person['name'] . '<br>';
}
```

## 条件付きSELECT文にチャレンジ(object_select1.php)

```text
    // もしも$_GET['uid']が空なら、uidを求めるフォームを表示(GETメソッド使用)
    if (empty($_GET['uid'])) {
    ?>
        <!-- 検索フォームを以下に記述 -->
        <form action="obj_select1.php" method="get">
            <label for="uid">ユーザIDを入力してください：</label>
            <input type="text" name="uid" id="uid">
            <input type="submit" value="検索">
        </form>
        <!-- ここまで -->
    <?php
    } else {
        // uidをキーにして、GETメソッドで受け取ったuidを代入
        $uid = $_GET['uid'];

        // DbPhpクラスのオブジェクト生成し、selectPerson( )メソッドをよびだす
        require_once __DIR__ . '/classes/dbphp.php';
        $dbPhp = new DbPhp();
        $person = $dbPhp->selectPerson($uid);

        // 抽出した結果に応じた画面を表示する
        if (empty($person)) {
            echo '該当するユーザがいませんでした(uid=' . $uid . ')';
        } else {
            echo 'uid=' . $person['uid'] . ', name=' . $person['name'] . '<br>';
        }
    }
    ?>
```