# 「入力フォーム」のチャレンジ問題

- [「入力フォーム」のチャレンジ問題](#入力フォームのチャレンジ問題)
  - [チャレンジ問題について](#チャレンジ問題について)
  - [解答例](#解答例)

## チャレンジ問題について

[「入力フォーム」の課題](../http-post-kadai/README.md)が早めに終わった方はこちらをチャレンジしてみてください。
(※あくまでチャレンジ問題ですので、評価には含まれません。)

- 『「入力フォーム」の課題』のcloneした、`public`ディレクトリに、`tour.html`、`tour.php`、`tour.css`作成し、以下のようにブラウザに表示される見積もりフォームを作成してください。

`tour.html`
![](./images/tour_html.png)

`tour.php`
![](./images/tour_php.png)

なお、`tour.css`は自力で作成する必要はありません。以下のソースコードをコピーして貼り付けてください。

`tour.css`
```css
@charset "UTF-8";
/* ページ全体を中央に表示(センタリング)するには、
ページ全体である<body>~</body>の中に<div>~</div>を入れ、
その<div>~</div>を中央に表示する。

tour.htmlとtour.phpでは、<body>~</body>の中に
<div id="main">~</div>を入れている。

次のbodyと#mainの2つの設定でセンタリングが実装される。
*/

/* body要素内の文字列をセンタリング */
body {
    text-align: center;
}

/* <div id="main">~</div>全体をセンタリング ・・ 先頭に「#」」をつけるとid属性を表す */
#main {
    margin: 0 auto;
    /* 上下のマージンを0、左右のマージンをauto(オート:自動)にすることで中央に配置される */
}

/* id属性がmainTableであるテーブルの境界線を1ピクセルとし、センタリング表示する */
#mainTable {
    border: 1px solid;
    margin: 0 auto;
}

/* テーブルのセル(<td>と</td>で囲まれた領域)の境界線を1ピクセル、文字列表示をデフォルトの左詰めにする */
td {
    border: 1px solid;
    text-align: left;
}

/* titleColorクラス属性を持つセルの背景色を指定する ・・ 先頭に「.」(ピリオド)をつけるとクラス属性を表す */
.titleColor {
    background-color: #a0d8ef;
}

/* centerPosクラス属性を持つセル内の文字列を中央に表示する */
.centerPos {
    text-align: center;
}

/* rightPosクラス属性を持つセル内の文字列を右詰めに表示する(数字の場合デフォルト) ・・ これも*/
.rightPos {
    text-align: right;
}
```

## 解答例

`tour.html`

```php
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>見積りフォーム</title>
    <link rel="stylesheet" href="tour.css">
</head>

<body>
    <div id="main">
        <h4>ハワイ旅行見積りフォーム</h4>
        <form method="POST" action="tour.php">
            <table id="mainTable">
                <tr>
                    <td class="titleColor">基本料金</td>
                </tr>
                <tr>
                    <td>
                        <input type="radio" name="course" value="1" checked>エコノミークラス:150,000円<br>
                        <input type="radio" name="course" value="2">ビジネスクラス:200,000円<br>
                        <input type="radio" name="course" value="3">LCC(ローコストキャリア)クラス:100,000円
                    </td>
                </tr>
                <tr>
                    <td class="titleColor">オプショナルツアー(複数選択可)</td>
                </tr>
                <tr>
                    <td>
                        <input type="checkbox" name="options[ ]" value="1">ポリネシア文化センター:5,000円<br>
                        <input type="checkbox" name="options[ ]" value="2">サンセットクルーズ:10,000円<br>
                        <input type="checkbox" name="options[ ]" value="3">キラウェア火山観光:40,000円
                    </td>
                </tr>
                <tr>
                    <td class="centerPos">
                        <input type="submit" value="見積り結果表示">&nbsp;&nbsp;<input type="reset" value="リセット">
                    </td>
                </tr>
            </table>
        </form>
    </div>
</body>

</html>
```

`tour.php`
```php
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>見積り結果</title>
    <link rel="stylesheet" href="tour.css">
</head>

<body>
    <div id="main">
        <h4>ハワイ旅行見積り結果</h4>
        <table id="mainTable">
            <?php
            $sum = 0;
            $course_names = ['エコノミークラス', 'ビジネスクラス', 'LCC(ローコストキャリア)クラス'];
            $course_prices = [150000, 200000, 100000];
            $option_names = ['ポリネシア文化センター', 'サンセットクルーズ', 'キラウェア火山観光'];
            $option_prices = [5000, 10000, 40000];

            // 基本料金の処理
            $course = $_POST['course'] - 1;
            $sum = $course_prices[$course];
            echo '<tr><td>' . $course_names[$course] . '</td>';
            echo '<td class="rightPos">&yen;' . number_format($course_prices[$course]) . '</td></tr>';

            // オプショナルツアーの処理
            if (isset($_POST['options'])) {
                foreach ($_POST['options'] as $option) {
                    echo '<tr><td>' . $option_names[$option - 1] . '</td>';
                    echo '<td class="rightPos">&yen;' . number_format($option_prices[$option - 1]) . '</td></tr>';

                    $sum += $option_prices[$option - 1];
                }
            }

            // 合計金額の処理
            echo '<tr><td class="titleColor">合計金額</td>';
            echo '<td class="titleColor rightPos">&yen;' . number_format($sum) . '</td></tr>';
            ?>
        </table>
        <a href="tour.html">見積り画面に戻る</a>
    </div>
</body>

</html>
```
