# 関数

- [関数](#関数)
  - [事前準備](#事前準備)
  - [PHPの2種類の関数](#phpの2種類の関数)
  - [組み込み関数](#組み込み関数)
    - [header](#header)
    - [isset、empty、is\_null](#issetemptyis_null)
    - [is\_numeric](#is_numeric)
  - [intval、floatval](#intvalfloatval)
    - [strlen、 mb\_strlen](#strlen-mb_strlen)
    - [substr、  mb\_substr](#substr--mb_substr)
    - [explode、 implode](#explode-implode)
    - [time、date、mktime](#timedatemktime)
    - [htmlspecialchars](#htmlspecialchars)
  - [ユーザー定義関数](#ユーザー定義関数)
  - [エスケープ処理のサンプルコード](#エスケープ処理のサンプルコード)

## 事前準備

[こちらのページ](https://classroom.github.com/a/HvLGxhXL)から、ソースコードを`C:¥xampp¥htdocs`へcloneすること。

```text
C:¥xampp¥htdocs
    └── 08_function-GitHubのユーザー名
        ├── <中略>
        ├── src
        |   ├── escape.html
        |   ├── escape.php
        |   ├── function.php
        |   ├── unescape.html
        |   └── unescape.php
        └── <中略>

```

## PHPの2種類の関数

1. 組み込み関数　・・・　プログラム言語としてPHPであらかじめ用意されている関数
1. ユーザー定義関数　・・・　ユーザー（プログラマー）が自分で作成する関数

<div style="page-break-before:always"></div>

## 組み込み関数

PHPには様々な関数があらかじめ用意されているが、これを最初から全部覚える必要はない。 次のサイトなどを参考に、必要に応じて自分で調べて利用できるようになればよい。

[PHPマニュアル　関数リファレンス](http://php.net/manual/ja/funcref.php)<br>
[PHP入門　PHP関数リファレンス](https://webkaru.net/php/function-reference/)<br>

以下、関数のサンプルを紹介していくが、動作確認用として「function.php」を活用すること。※提出物ではない。

### header

指定したページにリダイレクト（遷移）する。 自分のサイトのページだけでなく、外部のサイトにあるページへもリダイレクトできる。

構文:
```PHP
header( 'Location： リダイレクト先のURL' );
```

なお、リダイレクトする際には、それ以降のコードが実行されないように`exit( )`を指定して処理を終了する。<br>

【使用例】<br>
自分のサイトのページにリダイレクトする場合

```php
<?php
header('Location： another.php'); // リダイレクト先を相対パスでファイル名を指定 
exit( );
?> 
```

外部のサイトのページへリダイレクトする場合

```php
<?php
header('Location： https://www.kobedenshi.ac.jp/'); // リダイレクト先をhttp または https から指定 
exit( );
?>
```

<div style="page-break-before:always"></div>

### isset、empty、is_null

`isset ( )`<br>
変数がセットされており、それが NULL でないならば `TRUE` を返し、それ以外は `FALSE` を返す。`is_null( )` と反対の結果を返す。<br>

`empty ( )`<br>
変数が空(空文字、0、NULL、FALSE、空の配列)だったら `TRUE` それ以外は `FALSE` を返す。<br>

`is_null ( )`<br>
変数が NULL かどうか調べ、変数が NULL なら `TRUE`、それ以外は `FALSE` を返す。変数が未定義の時も `TRUE` を返すが、未定義のチェックは `isset( )` を使ったほうが良い。<br>

各関数の判定結果は次の通り。 なお、変数を宣言したが値を設定してない場合はNULLとなる。

||未定義|NULL|0|""(空文字)|"abc"|
| :- | - | - | - | - | - |
|isset ( )|FALSE|FALSE|TRUE|TRUE|TRUE|
|empty ( )|TRUE|TRUE|TRUE|TRUE|FALSE|
|is_null ( )|TRUE|TRUE|FALSE|FALSE|FALSE|

【使用例】

```php
<?php

if ( isset ( $_POST[ 'パラメータ名' ] ) ) {
// 指定したパラメータ名の値がセットされており、NULLでなければ、そのための処理を記述する 
}

?>
```

<div style="page-break-before:always"></div>

### is_numeric

変数に数字、または数字を表す数値文字列（ただし、半角であること）が格納されているかどうかを確認し、 数字または数字を表す数値文字列であればTRUEを返し、そうでなければFALSEを返す。

| |半角|半角|半角|半角|半角|全角|全角|
| - | - | - | - | - | - | - | - |
|値|123|123\.456|"abc"|"123"|"123.456"|"１２３"|"１２３．４５６"|
|結果|TRUE|TRUE|FALSE|TRUE|TRUE|FALSE|FALSE|

【使用例】

```php
<?php
$a  =  "123.456";
if ( is_numeric ( $a ) ） {
    echo   $a　． "は数値です。"; //  「123.456は数値です」と表示される 
}
?>
```

## intval、floatval

`intval( )`<br>
数値文字列を整数型の数値に変換する。キャスト演算子 `(int)` でも変換できる。

`floatval( )`<br>
数値文字列を浮動小数点型の数値に変換する。キャスト演算子 `(float)` でも変換できる。なお、`doubleval( )` は、`floatval( )` のエイリアス（別名）で、キャスト演算子 `(double)` も使える。

【使用例】
```php
<?php
$a  =  "123"; // 文字列で定義している
$b  =  "456"; // これも文字列で定義している 
echo   $a   +   $b; // 「579」と表示 ←注意
echo  intval( $a )  +  intval( $b ); // 「579」と表示
echo  $a  +  ( int ) $b; // 「579」と表示

$c  =  "12.3";
$d  =  "45.6";
echo  $c  +  $d; // 「57.9」と表示
echo  floatval( $c )  +  doubleval( $d); // 「57.9」と表示 
echo  ( float ) $c  +  ( double ) $d; // 「57.9」と表示
?>
```

`echo $a + $b;` は数値文字列の足し算なので、Javaのように「123456」と表示されるように思われるが、 PHPの場合、自動型変換の機能があるためこのような結果となる。 だからといって、手を抜いて数値文字列を型変換せずに四則演算しないように。

<div style="page-break-before:always"></div>

### strlen、 mb_strlen

`strlen ( )`<br>
指定した文字列の長さを取得する。マルチバイト文字列（つまり、日本語の文字列）の長さは正しく取得できない。

`mb_strlen( )`<br>
マルチバイト文字列も含めて指定した文字列の長さを取得する。（mbは"multibyte"の意味）

"UTF-8"、"EUC-JP"、"SJIS"のエンコーディングを指定できる。エンコーディングの指定を省略した場合、PHPのシステム環境による。

【使用例】

```php
<?php
$string1 = "php";
echo  strlen( $string1 ); // 3 正しい 
echo  mb_strlen( $string1 , "UTF-8" ); // 3 正しい

$string2 = "php入門";
echo  strlen( $string2 ); // 9 正しくない
echo  mb_strlen( $string2, "UTF-8" ); // 5 正しい文字列の長さ
?>
```

### substr、  mb_substr

`substr( )` と `mb_substr( )`は、どちらも指定した文字列の一部分を取得する関数だが、`strlen( )` と `mb_strlen( )` 同様に、マルチバイト文字列に対応しているのは `mb_substr( )` なので、これの使い方を示す。

構文：

```php
mb_substr ( 対象文字列,  開始位置 [, 取得する文字数 [ , エンコーディング] ] )
```

開始位置: 文字列の1文字目から取得する場合は「0」を指定する。<br>
エンコーディング: "UTF-8"、"EUC-JP"、"SJIS"を指定できる。省略した場合、PHPのシステム環境による。<br>

【使用例】

```php
<?php

echo mb_substr("PHP入門",  0,  3,  "UTF-8"); // 「PHP」と表示 
echo mb_substr("PHP入門",  3,  2,  "UTF-8"); // 「入門」と表示

?>
```

<div style="page-break-before:always"></div>

### explode、 implode

`explode( )`<br>
指定した区切り文字列でデータを分割し、配列に格納する。

`implode( )`<br>
配列の要素を指定した区切り文字列で連結した文字列に変換する。

【使用例】

```php
<?php
$string3 = "a, b, c, d, e";
$string4 = explode("," ,  $string3); // 区切り文字列として「,」を指定し、配列に変換 
foreach ( $string4  as  $string ) {
    echo $string . "&nbsp;"; // 「a b c d e」と表示
}

$string5 = ["a", "b", "c", "d", "e"];
echo implode(",",  $string5); // 「a,b,c,d,e」と表示
?>
```

`explode( )` も `implode( )` も、第1引数の記号は「カンマ区切り」以外のものも使用できる。

### time、date、mktime

`time ( )`<br>
現在の時刻をUnixエポック（1970年1月1日 00:00:00 GMT）からのタイムスタンプ（通算秒）として取得する。

`date ( )`<br>
指定したタイムスタンプをフォーマットする。タイムスタンプが指定されていない場合は現在時刻( `time( )` の値）をフォーマットする。

構文：
```php
date ( フォーマット [ , タイムスタンプ]  )
```

`mktime ( )`<br>
年月日時分秒を指定して、そのタイムスタンプを取得する。特定の日時を取得するときに利用する。

構文:

```php
mktime ([時 [ , 分 [ , 秒 [ , 月 [ , 日 [, 年 ] ] ] ] ] ] ) 
```

引数は右から省略可能で、省略した場合は現在の値が使われる。

<div style="page-break-before:always"></div>

フォーマットはいろいろなものがあり、必要に応じて使い分ける。

|フォーマット(年)|説明|例|
| - | - | - |
|Y|4桁の西暦|2020|
|y|2桁の西暦|20|
|L|閏年の判定（1=閏年、0=平年）|0または1|

|フォーマット(月)|説明|例|
| - | - | - |
|m|2桁の数字（1桁の月は先頭に0をつける） |01 ～ 12|
|n|1桁または2桁の数字|1 ～ 12|
|**t**|指定した月の日数（月末日）|28 ～ 31|

|フォーマット(日)|説明|例|
| - | - | - |
|d|2桁の数字（1桁の日は先頭に0をつける）|01 ～ 31|
|j|1桁または2桁の数字|1 ～ 31|

|フォーマット(時)|説明|例|
| - | - | - |
|H|24時間単位で2桁の数字（1桁の時は先頭に0をつける） |00 ～ 23|
|G|24時間単位で1桁または2桁の数字 |0 ～ 23|
|h|12時間単位で2桁の数字（1桁の時は先頭に0をつける） |00 ～ 12|
|g|12時間単位で1桁または2桁の数字|0 ～ 12|

|フォーマット(分)|説明|例|
| - | - | - |
|i|2桁の数字|00 ～ 59|

「分」には「0なし」でのフォーマットがない。必要な場合は `intval( )` で数値化する。 `date( "i" )` が「05」を返す時、`intval( date( "i" ) )` で「5」を求める

|フォーマット(秒)|説明|例|
| - | - | - |
|s|2桁の数字|00 ～ 59|

「秒」にも「0なし」でのフォーマットがない。必要な場合は `intval( )` で数値化する。`date( "s" )` が「09」を返す時、`intval( date( "s" ) )` で「9」を求める。

|フォーマット(曜日)|説明|例|
| - | - | - |
|w|曜日番号（0=日、1=月、2=火、3=水、4=木、 5=金、6=土）|0 ～ 6|

日本語の曜日を求める場合は、次を利用して、`$day_of_week[ date( "w" ) ]` とすると曜日が求められる。

```php
$day_of_week  =  ["日", "月", "火", "水", "木", "金", "土"]; 
```

【使用例】 2023年5月9日に実行した場合

```php
<?php

// 現在の年月日時分秒を取得する
echo  date ( "Y年m月d日 H時i分s秒"); // 2023年05月09日 08時06分02秒

 // 現在の1時間後の年月日時分秒を取得する
echo  date ( "Y年m月d日 H時i分s秒", time( ) + 60 * 60); // 2023年05月09日 09時06分02秒 

// 特定の時分秒（14時43分39秒）を取得する
echo  date ( "Y年m月d日 H時i分s秒", mktime(14, 43, 39)); // 2023年05月09日 14時43分39秒

// 現在の30日後の年月日を取得する（時分秒は0）
echo  date ( "Y年m月d日 H時i分s秒", mktime(0, 0, 0, date("m"), date("d") + 30, date("Y") ));// 2023年06月08日 00時00分00秒 

// 今月の月末日を取得する ・・ 2通りの方法がある

echo  date ( "Y年m月t日"); // 2023年05月31日

echo  date ( "Y年m月d日", mktime(0, 0, 0, date("m") + 1, 0, date("Y"))); // 2023年05月31日([注]data("m") + で「6」月、その「0」日なので、「5月31日」となる)

?> 
```

<div style="page-break-before:always"></div>

### htmlspecialchars

フォームから送られてきた値や、データベースから取り出した値をブラウザ上に表示する際に使用する。 クロスサイトスクリプティングなど悪意のあるコードの埋め込みを防ぐ。(エスケープやサニタイズと呼ばれる)

構文:
```php
htmlspecialchars ( エスケープする文字列, エスケープの種類, 文字コード );
```

第1引数：エスケープする文字列は、ブラウザ上に表示する値を指定する。<br> 第2引数：エスケープの種類はいくつかあるが、「ENT_QUOTES」を指定する。<br>
これにより次の文字が特殊文字（実態参照）に変換される。

|変換前|変換後|
| - | - |
|& （アンパサンド）|`&amp;`|
|" （ダブルクォート）|`&quot;`|
|' （シングルクォート）|`&#039;`または `&apos;`|
|< （小なり）|`&lt;`|
|> （大なり）|`&gt;`|

第3引数：文字コードは「UTF-8」を指定する。

【使用例】

```php
<?php

$new = "<a href='test'>Test</a>";
echo htmlspecialchars ( $new,  ENT_QUOTES,  "UTF-8");

?>
```

`<a href='test'>Test</a>` が `&lt;a href=&#039;test &#039;&gt;Test&lt;/a &gt;` に変換されるが、ブラウザには `<a href='test'>Test</a>` と表示される。

【重要】授業のサンプルでは、この処理を省略しているが、実際のWebアプリケーションを作成するときは 必ずこの関数を使用し、値をエスケープしてブラウザ上に表示すること。

<div style="page-break-before:always"></div>

## ユーザー定義関数

ユーザー定義関数とは、ユーザー（プログラマー）自身が作成する関数。
ユーザー定義関数の基本文法は次の通り。

```php
function  関数名 ( 引数リスト ) {
    具体的な処理を記述
    return  戻り値； // 戻り値がある場合（なければ記述不要）
}
```

戻り値がない関数

```php
function  func1 ( $a, $b ) { // func1( )関数を呼び出すだけで計算結果を表示する
    echo  $a . ' + ' . $b . ' = ' . ( $a + $b );
}
```

戻り値がある関数

```php
function  func2 ( $a, $b ) { // func2( )関数を呼び出すと計算結果が戻り値として返ってくる
    return  $a + $b;
}
```

【使用例】

```php
<?php

// 関数func1( )の定義
function  func1 ( $a, $b ) {
    echo  $a . ' + ' . $b . ' = ' . ( $a + $b );
}

// 関数func2( )の定義 
function  func2 ( $a, $b ) {
    return  $a + $b;
}

$a = 10; $b = 25;
// 関数func1( )を呼び出す
func1( $a, $b); // 10 + 25 = 35　が計算され「35」が表示される

// 関数func2( )を呼び出す
$c = func2($a, $b); // $c に戻り値である「35」 が代入される
echo  $a . ' + ' . $b . ' = ' . $c; // 「10 + 25 = 35」と表示される
?>
```

<div style="page-break-before:always"></div>

「組み込み関数」の「htmlspecialchars」でセキュリティ対策のため実際のWebアプリケーション作成時には 送信されてきたデータをブラウザに表示する場合には必ず実行すると紹介した。 しかしながら、この関数の文字列が長いので、いちいち記述するのが面倒なため、この関数を関数化して 次のように利用する方法がよくとられる。

```php
<?php

function  h ( $data ) { // 関数h を定義
    return  htmlspecialchars ( $data,  ENT_QUOTES,  "UTF-8"); 
}

echo  h ( $_POST['name'] ); // 関数hを実行、つまりhtmlspecialschars( )関数を実行

?>
```

<div style="page-break-before:always"></div>

## エスケープ処理のサンプルコード

ここからは、エスケープ処理を通して、組み込み関数(htmlspecialchars)とユーザー定義関数を実装する。

1. unescape.html と unescape.php を作成する.<br>
**unescape.html**<br>
![](./images/08/unescape_html.png)<br>
**unescape.php**<br>
![](./images/08/unescape_php.png)<br>

<div style="page-break-before:always"></div>

2. ブラウザで unescape.html にアクセスし、氏名として次の２つの値を入力して送信してみる。

- 神戸電子
- `<body  onload="alert('Hack, Now!!');">`<br><br>
「神戸電子」を送信した場合<br>
![](./images/08/unescape_html_display_text.png)<br>
![](./images/08/unescape_php_display_text.png)<br><br>

<div style="page-break-before:always"></div>

「`<body  onload="alert('Hack, Now!!');">`」を送信した場合<br>
![](./images/08/unescape_html_display_javascript.png)<br><br>

![](./images/08/unescape_php_display_dialog.png)<br><br>
**JavaScriptのコードが実行されてしまう。** ここでは、単にダイアログを表示しているだけだが、これを悪用すると不正プログラムの感染、ユーザを騙す表示によるフィッシング詐欺、クッキー情報の取得によるセッションハイジャック、情報詐取、他の不正サイトへの誘導、などの被害に繋がる。
![](./images/08/unescape_php_display_javascript.png)<br>
この画面の何も書かれていない場所で右クリックし、「ページのソースを表示」させた画面を次に示す。入力されたJavaScriptのコードが確認できる。<br>
![](./images/08/unescape_php_display_source.png)<br>

<div style="page-break-before:always"></div>

3. そこで、JavaScriptのコードが実行されないように修正する。以下の escape.html と escape.php を作成する。<br><br>
**escape.html**<br>
![](./images/08/escape_html.png)<br>
**escape.php**<br>
![](./images/08/escape_php.png)<br>

<div style="page-break-before:always"></div>

4. ブラウザで escape.html にアクセスし、`<body  onload="alert('Hack, Now!!');">` を入力して送信する。<br>
![](./images/08/escape_html_display.png)<br>
![](./images/08/escape_php_display.png)<br>
**JavaScriptのコードが実行されずに、テキストして画面に表示されている。** この画面の何も書かれていない場所で右クリックし、「ページのソースを表示」させた画面を次に示す。

<div style="page-break-before:always"></div>


![](./images/08/escape_php_display_source.png)<br>

エスケープ処理されたJavaScriptのコードが確認できる。エスケープ処理された特殊文字については、以下の表を参照。<br><br>
|変換前|変換後|
| - | - |
|& （アンパサンド）|`&amp;`|
|" （ダブルクォート）|`&quot;`|
|' （シングルクォート）|`&#039;`または `&apos;`|
|< （小なり）|`&lt;`|
|> （大なり）|`&gt;`|

**本章「08_関数」では課題の提出はございません。**
