﻿# 仕様書⑥ : 注文画面

- [仕様書⑥ : 注文画面](#仕様書--注文画面)
  - [事前準備](#事前準備)
  - [本章から追加されたテーブルについて](#本章から追加されたテーブルについて)
  - [注文画面(order\_now.php)の作成](#注文画面order_nowphpの作成)
  - [注文に関するデータベース操作を行うクラスOrder](#注文に関するデータベース操作を行うクラスorder)
  - [Cart.classに、カート内の全ての商品を削除するclearCartメソッドを作成](#cartclassにカート内の全ての商品を削除するclearcartメソッドを作成)
  - [注文画面(order\_now.php)](#注文画面order_nowphp)
  - [ディレクトリ構成の確認](#ディレクトリ構成の確認)
  - [動作確認](#動作確認)
  - [課題の作成と提出](#課題の作成と提出)
    - [採点について](#採点について)
    - [課題の合格基準](#課題の合格基準)
    - [合格確認方法](#合格確認方法)

## 事前準備

1. [こちらのページ](https://classroom.github.com/a/GoqTTjYE)から、ソースコードを`C:¥web_app_dev`へcloneしてください。

2. **「仕様書⑤」までで作成した`public`ディレクトリ内のソースコードを、今回colneした`public`ディレクトリにコピーしてください。**
ソースコードをコピーすると、以下のようなディレクトリ構成となります。

```text
public
├── cart
│   ├── cart_add.php
│   ├── cart_change.php
│   ├── cart_delete.php
│   └── cart_list.php
├── classes
│   ├── cart.php
│   ├── dbdata.php
│   └── product.php
├── css
│   └── minishop.css
├── images
├── index.php
└── product
    ├── product_detail.php
    └── product_select.php
```

## 本章から追加されたテーブルについて

前章で作成されたテーブルitems, cartに加え、新たにテーブルorders, orderdetailsを作成します。

**テーブル名：orders**

| カラム名 | データ型 | 制約 | 備考 |
| - | - | - | - |
|orderId|int型|主キー、auto_increment|注文番号|
|orderdate|datetime型||注文日時|

**テーブル名：orderdetails**

| カラム名 | データ型 | 制約 | 備考 |
| - | - | - | - |
|orderId|int型|主キー|注文番号(テーブルordersのorderId)|
|itemId|int型|主キー|商品番号|
|quantity|int型||注文数|

※primary keyは「orderId」と「itemId」の複数列を使った複合主キーとします。

## 注文画面(order_now.php)の作成

カート内の商品画面(cart_list.php)の「注文する」リンクをクリックすると、

- 注文テーブルordersに注文日時を登録し、注文番号を取得する
- 注文明細テーブルorderdetailsに注文番号と個別の商品情報を登録する
  
の処理を行い、注文した商品の内容を注文画面(order_now.php)に表示します。

**カート内の商品画面：cart_list.php**<br>
![](./images/cart_list_display.png)

**注文画面：order_now.php**<br>
![](./images/order_now_display.png)

この一連の処理を以下の手順で実装していきます。

1. データベースに関わる処理は、classes/order.php のクラス`Order` に以下のメソッドとして定義する<br>
  `addOrder`メソッド ・・・ カート内のすべての商品を注文内容として登録するメソッド

1. カート内の商品画面(cart_list.php)から送られてくるリクエストを受け取る注文画面(order_now.php)には、以下の処理を記述する
   1. クラス`Cart`のオブジェクトを生成し、カート内のすべての商品を取り出すメソッドを呼び出す
   2. クラス`Order`のオブジェクトを生成し、カート内のすべての商品を注文内容として登録するメソッドを呼び出す
   3. 取り出したカート内のすべての商品を画面に表示する(金額と合計金額も表示する)
   4. Cartオブジェクトの全ての商品を削除するメソッドを呼び出し、カート内の全ての商品を削除する

## 注文に関するデータベース操作を行うクラスOrder

データベースの基本事項を定義するクラス`DbData`を継承し、注文に関するデータベース操作を行うクラス`Order`を定義します。<br>
定義するファイルは、classes/order.php で、このクラス`Order`にカート内のすべての商品を注文内容として登録する`addOrder`メソッドを以下の仕様で定義します。<br>

```text
アクセス修飾子： public
メソッド名： addOrder
引数： $cartItems(カート内の全ての商品を取り出した結果セット)
戻り値： なし
```

このメソッド内の具体的な処理は以下のとおりです。

1. 注文テーブルに注文日時をセットし、注文データとして登録する
2. 登録した注文データの注文番号を取り出す
3. 引数で受け取った$cartItemsの各商品に注文番号をセットしたデータを注文明細テーブルに登録する

【ヒント】

- 注文テーブルにセットする注文日時は以下のフォーマットを用いれば、MySQLのDATETIME型と一致する<br>
**date("Y-m-d H:i:s")**
- 登録した注文データの注文番号は「auto_increment」で採番されるので、以下の処理で取得することができる

```php
// last_insert_id( )は、直前にinsertされたデータのauto_incrementの値を返すMySQLの関数
$sql = "SELECT last_insert_id( ) FROM orders";
$stmt = $this->query( $sql,  [ ] );
$result = $stmt->fetch( );
$orderId = $result[ 0 ]; // auto_incrementで登録された注文番号(orderId)の値を取得する
```

## Cart.classに、カート内の全ての商品を削除するclearCartメソッドを作成

受け取る引数も戻り値もなく、テーブルcartにあるすべての商品を削除する「全商品削除メソッド」を以下の仕様で定義します。

```text
アクセス修飾子: public
メソッド名: clearCart
引数: なし
戻り値: なし
```

## 注文画面(order_now.php)

この画面の完成形は以下のとおり。<br>
![](./images/order_now_display.png)

この画面と以下の処理概要を参考に、order/order_now.php を作成してください。

1. Cartオブジェクトを生成する
2. Cartオブジェクトの`getItems`メソッドを呼び出し、カート内のすべての商品を取り出す
3. Orderオブジェクトを生成する
4. Orderオブジェクトの`addOrder`メソッドを呼び出し、データベースに登録する
5. 取り出したカート内のすべての商品を画面に表示する(金額と合計金額も表示する)
6. Cartオブジェクトの`clearCart`メソッドを呼び出し、カート内の全ての商品を削除する

## ディレクトリ構成の確認

動作確認をする前に、ディレクトリ構成が以下のようになっていることを確認してください。

```text
public
├── cart
│   ├── cart_add.php
│   ├── cart_change.php
│   ├── cart_delete.php
│   └── cart_list.php
├── classes
│   ├── cart.php
│   ├── dbdata.php
│   ├── order.php ←本章で追加
│   └── product.php
├── css
│   └── minishop.css
├── images
├── index.php
└── order
│   └── order_now.php ←本章で追加
└── product
    ├── product_detail.php
    └── product_select.php
```

## 動作確認

カートに複数の商品を入れ、カート内の商品画面(cart_list.php)の「注文する」リンクをクリックすると 注文画面(order_now.php)にカート内の全ての商品が表示され、データベースの注文テーブルと 注文明細テーブルにデータが登録されていることを確認する。

カート内の商品画面：cart_list.php<br>
「注文する」リンクをクリックする<br>
![](./images/cart_list_display.png)<br><br>

注文画面：order_now.php<br>
![](./images/order_now_display.png)

注文テーブル(orders)と注文明細テーブル(orderdetails)には、以下のようにデータが追加されます。
また、カートテーブル(cart)は空になっています。

- 注文テーブル: **orders**<br>
  ![](./images/orders.png)
  
- 注文明細テーブル: **orderdetails**<br>
  ![](./images/orderdetails.png)

- カートテーブル: **cart**<br>
  ![](./images/cart.png)

## 課題の作成と提出

### 採点について

提出した課題はGitHub上で自動採点されます。
提出後、課題が合格しているかを確認してください。
合格していない場合は修正後pushし、再提出してください。

### 課題の合格基準

以下の2つを合格基準とします。

1. 注文処理が実行されていること
   1. ordersテーブルに注文データが登録されていること
   2. orderdetailsテーブルに注文明細データが登録されていること
2. カート内の商品が削除されていること

### 合格確認方法

1. 本課題の[課題ページ](https://classroom.github.com/a/GoqTTjYE)に再度アクセスします。
2. 画面上部にある`Actions`をクリックしてください。<br>
![](./images/acions.png)
1. **一番上**の行に、緑色のチェックが入っていればOKです。<br>
![](./images/pass.png)