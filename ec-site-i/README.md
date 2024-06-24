# ミニショップ仕様書①

- [ミニショップ仕様書①](#ミニショップ仕様書)
  - [概要](#概要)
  - [画面遷移図](#画面遷移図)
  - [商品に関する機能の詳細画面](#商品に関する機能の詳細画面)
    - [ジャンル選択](#ジャンル選択)
    - [ジャンル別商品一覧](#ジャンル別商品一覧)
    - [商品詳細表示](#商品詳細表示)
  - [事前準備](#事前準備)
  - [ジャンル選択画面とジャンル別商品一覧画面の作成](#ジャンル選択画面とジャンル別商品一覧画面の作成)
    - [データベースの作成](#データベースの作成)
    - [ヘッダーとフッター](#ヘッダーとフッター)
    - [ジャンル選択画面（index.php）](#ジャンル選択画面indexphp)
    - [データベースの基本事項を定義するDbData.class](#データベースの基本事項を定義するdbdataclass)
    - [商品データを操作するProduct.class](#商品データを操作するproductclass)
    - [ジャンル別商品一覧画面（product\_select.php）](#ジャンル別商品一覧画面product_selectphp)
    - [動作確認](#動作確認)

## 概要

PHPのオブジェクト指向プログラミングの要素を取り入れ、簡易版ショッピングサイト(ミニショップ)を作成する。

## 画面遷移図

<img src="./images/13/screen.png" width="75%">

## 商品に関する機能の詳細画面

### ジャンル選択

- パソコン、ブック、ミュージックの３つのジャンルをラジオボタンで表示する
- ブックにデフォルトチェックが入っている

### ジャンル別商品一覧

- 選択されたジャンルの商品概要を一覧で表示する
- 各商品にはそれぞれの詳細画面へのリンクがある

### 商品詳細表示

- ジャンル別商品一覧から選択された商品の詳細画面を表示する
- 注文数のプルダウンメニューと カートに入れるためのボタンがある

## 事前準備

[こちらのページ](https://classroom.github.com/a/4ZOKphQa)から、ソースコードを`C:¥xampp¥htdocs`へcloneすること。

```text
C:¥xampp¥htdocs
    └── 13-minishop-GitHubのユーザー名
        └── src
            ├── cart
            │   ├── cart_add.php      ←カートに商品を入れる処理を行う
            │   ├── cart_change.php   ←カート内の商品の注文数を変更する処理を行う  
            │   ├── cart_clear.php    ←カート内のすべての商品を削除する処理を行う
            │   ├── cart_delete.php   ←カート内の特定の商品を削除する処理を行う
            │   └── cart_list.php     ←カート内の商品を画面に表示する
            ├── classes
            │   ├── cart.php    ←「カート」に関するCart.classを定義する
            │   ├── dbdata.php  ←データベースの基本事項を定義する
            │   ├── order.php   ←「注文」に関するOrder.calss を定義する
            │   └── product.php ←「商品」に関するProduct.class を定義する
            ├── css
            │   └── minishop.css  ←このファイルはこのまま使用する
            ├── data
            │   ├── mysql_minishop.txt  ← 「商品」に関するDBのスクリプト
            │   ├── mysql_minishop_cart.txt ← 「カート」に関するDBのスクリプト
            │   └── mysql_minishop_order.txt  ← 「注文」に関するDBのスクリプト
            ├── footer.php        ←画面のフッターを構成する
            ├── header.php        ←画面のヘッダーを構成する
            ├── images            ←画像ファイルが入っているフォルダ
            ├── index.php         ←３つのジャンルから１つのジャンルを選択する画面
            ├── order
            │   └── order_now.php ←注文処理を行い、注文内容を表示する
            └── product
                ├── product_detail.php    ←特定の商品の詳細内容を表示する
                └── product_select.php    ←選択されたジャンルの商品一覧を表示する

```

## ジャンル選択画面とジャンル別商品一覧画面の作成

ジャンル選択画面 ： **index.php**

- パソコン、ブック、ミュージックの３つのジャンルをラジオボタンで表示する
- ブックにデフォルトチェックが入っている

<img src="./images/13/index_display.png" width="70%">

ジャンル選択画面（index.php）のジャンルを選択し、「選択」ボタンをクリックすると、 ジャンル別商品一覧画面（product_select.php）が表示される。

ジャンル別商品一覧画面 ： **product\_select.php**

- 選択されたジャンルの商品概要を一覧で表示する
- 各商品にはそれぞれの詳細画面へのリンクがある

<img src="./images/13/product_select_display.png" width="80%">

### データベースの作成

dataフォルダ内の **mysql\_minishop.txt** を使用してデータベースを作成する。すべてのコードは記述済みであるのでそのまま使用できるが、このファイルに書かれているコードに目を通し、データベースとそのテーブルの構成・構造をしっかり理解し、これからのプログラミングに臨むこと。

以下にコードの一部を抜粋する。

```sql
# データベースminishopの作成
set names utf8;
drop database if exists minishop;
create database minishop character set utf8 collate utf8_general_ci;

# ユーザーminiに、パスワードshopを設定し、データベースminishopに対するすべての権限を付与
grant all privileges on minishop.* to mini@localhost identified by 'shop';

# データベースminishopを使用する
use minishop;

# テーブルitemsの作成
create table items(
	ident	int auto_increment primary key,
	name	varchar(50) not null,
	maker	varchar(50) not null,
	price	int,
	image	varchar(20),
  genre varchar(10)
);

# テーブルitemsへデータを入力
insert into items(name, maker, price, image, genre)
	values('NEC LAVIE', 'NEC', 61980, 'pc001.jpg', 'pc');
    
 ＜ 後 略 ＞

```

### ヘッダーとフッター

画面のヘッダーとフッダーを構成するもの。すべての画面でプログラムが統一されているため、別ファイル(header.php, footer.php)で作成し、各画面では `require_once` を用い別ファイル(header.php, footer.php)を読み取ることでヘッダー、フッター部を実装する。

**header.php**

![](./images/13/header_code.png)

**footer.php**

![](./images/13/footer_code.png)

### ジャンル選択画面（index.php）

**index.php** のコードは次の通り。（実態はHTMLのみ）

![](./images/13/index_code.png)

①: `<form method="POST" action="product/product_select.php">`

データの送信先は「product_select.php」とする。

②: `<label><input type="radio" name="genre" value="pc">パソコン</label>&nbsp;&nbsp;`

`<label></label>`タグでラジオボタンを囲むことにより、「パソコン」という文字をクリックしてもラジオボタンが選択される効果を発揮する。

### データベースの基本事項を定義するDbData.class

このミニショップ用のDbData.classを作成して使用する。 `$dsn`、`$user`、`$password` の値がミニショップ用になっている以外は「オブジェクト指向プログラミング」 で作成した「DbData.class」と同じ内容。

**dbdata.php** のコードは次の通り。

![](./images/13/dbdata_code.png)

### 商品データを操作するProduct.class

データベースの基本事項を定義する「DbData.class」を継承し、商品データを操作する「**Product.class**」を 定義する。

このミニショップ全体では、このほかにカート内の商品を操作する「**Cart.class**」と注文を処理する 「**Order.class**」を定義する。

なお、それぞれのクラスを定義するPHPファイルは次の通り。（すべて空ファイルを配布済み）

Product.class src\classes\product.php

Cart.class src\classes\cart.php

Order.class src\classes\order.php

今回は、選択されたジャンルの商品データを抽出するメソッドを次の条件でこのProduct.class に定義する。

```text
アクセス修飾子： public
メソッド名： getItems
引数： $genre （選択されたジャンル）
戻り値： 抽出した商品データの結果セット
```

**product.php** のコードは次の通り。

![](./images/13/product1_code.png)

＊この `getItems( )` メソッドを、「product_select.php」 で呼び出して利用する。（そのコードは次に示す）

### ジャンル別商品一覧画面（product_select.php）

ジャンル選択画面（index.php）から選択されたジャンルのデータは、このジャンル別商品一覧画面 （product_select.php）が受け取るが、ここで処理する内容は次の通り。

1. 送られてきたジャンルの値を受け取る。
1. Product.class のオブジェクトを生成する
1. Productオブジェクトの `getItems( )` メソッドを呼び出し、抽出した商品データの結果セットを受け取る
1. 商品データの結果セットから商品を取り出し一覧画面を作成する

この **product_select.php** のコードは次の通り。

![](./images/13/product_select1_code.png)

**`<td>`セル内のクラス設定について**

各セルの幅や高さ、左詰め・右詰め・中央ぞろえなどといった表示に関する設定を行っている。具体的な設定は、src\css\minishop.css に記述してある。

①: `'<td class="td_mini_img"><img class="mini_img" src="../images/' . $item['image'] . '"></td>' .`

`class="td_mini_img"` の効果は 「minishop.css」に定義されており、画像を表示するセルのサイズを幅・高さともに40pxとしている。`class="mini_img"` の効果も「minishop.css」に定義されており、画像の大きさをセルのサイズに合わせる。<br><br>

②: `'<td class="td_item_name">' . $item['name'] . '</td>' .`

`class="td_item_name"` の効果は「minishop.css」に定義されており、商品名を表示するセルの横幅を200pxとし、収まりきらない場合は折り返して表示する。<br><br>

③: `'<td class="td_item_maker">' . $item['maker'] . '</td>' .`

`class="td_item_maker"` の効果は「minishop.css」に定義されており、メーカー等を表示する。セルの横幅を150pxとし、収まりきらない場合は折り返して表示する。<br><br>

④-1: `'<td class="td_right">&yen;' . number_format($item['price']) . '</td>' .`

`class="td_right"` の効果は「minishop.css」に定義されており、セル内のデータを右詰で表示する。<br><br>

④-2: `'<td class="td_right">&yen;' . number_format($item['price']) . '</td>' .`

`&yen;` は 「¥」 を表し、`number_format( )` 関数は、数字を3桁ごとにカンマ区切りにする関数。これにより「2678」を「¥2,678」と表示している。<br><br>

⑤: `'<td><a href="product_detail.php?ident=' . $item['ident'] . '">詳細</a></td>' .`

各商品の「詳細」の文字には、詳細画面を表示する「product_detail.php」へのリンクが張られている。そして、「product_detail.php」 にはクエリパラメータ `$ident` でその商品の商品番号を送信する。

### 動作確認

以上の作業終了後、次のように画面が表示されることを確認する。

- 最初に「ジャンル選択」画面にアクセスする。<br>
![](./images/13/index_display.png)

- ジャンルを選択し「選択」ボタンをクリックするとその「ジャンル別商品一覧」画面が表示される。<br>
＊図は「ブック」のジャンルを選択した場合。それぞれのジャンルで正しく商品一覧が表示されることを確認する。<br>
![](./images/13/product_select_display.png)

- 「パソコン」を選んだ場合<br>
![](./images/13/product_select_display_pc.png)

- ミュージックを選んだ場合<br>
![](./images/13/product_select_display_music.png)

**ミニショップの商品に関する機能はまだ完成ではありません。まだpushはしないでください。**
