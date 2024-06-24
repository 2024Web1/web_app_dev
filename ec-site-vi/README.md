# ミニショップ6

- [ミニショップ6](#ミニショップ6)
  - [事前準備](#事前準備)
  - [注文画面の作成](#注文画面の作成)
  - [注文テーブル「orders」と注文明細テーブル「orderdetails」](#注文テーブルordersと注文明細テーブルorderdetails)
    - [注文テーブル「orders」](#注文テーブルorders)
    - [注文明細テーブル「 orderdetails 」](#注文明細テーブル-orderdetails-)
  - [注文に関するデータベース操作を行う Order.class](#注文に関するデータベース操作を行う-orderclass)
  - [Cart.classに、カート内の全ての商品を削除するclearCart()メソッドを作成](#cartclassにカート内の全ての商品を削除するclearcartメソッドを作成)
  - [注文画面（order\_now.php）](#注文画面order_nowphp)
  - [動作確認](#動作確認)
  - [課題の提出と採点について](#課題の提出と採点について)
    - [課題の合格基準](#課題の合格基準)
    - [合格確認方法](#合格確認方法)
    - [エラーが出た時の対処法](#エラーが出た時の対処法)
    - [タイムアウトになっていないかを確認する](#タイムアウトになっていないかを確認する)
    - [プログラムが正確に書かれているか確認する](#プログラムが正確に書かれているか確認する)
      - [どこでエラーがでているか確認する](#どこでエラーがでているか確認する)
      - [プログラムが正確に書かれているか確認する](#プログラムが正確に書かれているか確認する-1)
  - [GitHub上での採点についてのお願い](#github上での採点についてのお願い)

## 事前準備

1. [こちらのページ](https://classroom.github.com/a/Tb1Lz6vU)から、ソースコードを`C:¥xampp¥htdocs`へcloneすること。

2. **前回の「14.ミニショップ3,4, 5」までで作成したソースコードを、今回colneしたソースコードに上書きすること。** ※ちなみに「14.ミニショップ3, 4, 5」までで作成したソースコードは次のとおりである。

```text
src
├── cart
│   ├── cart_add.php
│   ├── cart_change.php
│   ├── cart_delete.php
│   └── cart_list.php
├── classes
│   ├── cart.php
│   ├── dbdata.php
│   └── product.php
├── footer.php
├── header.php
├── index.php
└── product
    ├── product_detail.php
    └── product_select.php
```

## 注文画面の作成

カート内の商品画面（cart_list.php）の「注文する」リンクをクリックすると、データベースminishopの

- 注文テーブルordersに注文日時を登録し、注文番号を取得する
- 注文明細テーブルorderdetailsに注文番号と個別の商品情報を登録する
  
の処理を行い、注文した商品の内容を注文画面（order_now.php）に表示する。

**カート内の商品画面：cart_list.php**</br>
![](./images/15/cart_list_display.png)

**注文画面：order_now.php**</br>
![](./images/15/order_now_display.png)

<div style="page-break-before:always"></div>

この一連の処理を次の手順で実装していく。

```text
1. 注文内容を登録する2つのテーブルを新たに作成する。
注文テーブル「orders」と注文明細テーブル「orderdetails」

2. データベースに関わる処理は、src\classes\order.php の Order.class に次のメソッドとして定義する。
  メソッド名：addOrder( ) ・・・ カート内のすべての商品を注文内容として登録するメソッド

3. カート内の商品画面（cart_list.php）から送られてくるリクエストを受け取る「注文画面」（order_now.php）には、次の処理を記述する。

  (1) Cartクラスのオブジェクトを生成し、カート内のすべての商品を取り出すメソッドを呼び出す。
  (2) Orderクラスのオブジェクトを生成し、カート内のすべての商品を注文内容として登録するメソッドを呼び出す。
  (3) 取り出したカート内のすべての商品を画面に表示する。（金額と合計金額も表示する）
  (4) Cartオブジェクトの全ての商品を削除するメソッドを呼び出し、カート内の全ての商品を削除する。
```

## 注文テーブル「orders」と注文明細テーブル「orderdetails」

注文内容は、注文テーブル「orders」 と注文明細テーブル「orderdetails」の構成で登録する。

### 注文テーブル「orders」

|項目名|データ型|説明|その他|
| - | - | - | - |
|orderId|int型|注文番号|auto_increment,   primary key|
|orderdate|datetime型|注文日時|注文時の日時（年月日 時分秒）|

（＊）MySQLのDATETIME型のフォーマットは「YYYY-MM-DD HH:MM:SS」である。（時間表記は、00～23の24時間制）

### 注文明細テーブル「 orderdetails 」

|項目名|データ型|説明|その他|
| - | - | - | - |
|orderId|int型|注文番号|ordersテーブルのorderId|
|itemId|int型|商品番号||
|quantity|int型|注文数||

（＊）primary keyは「orderId」と「itemId」の複数列を使った複合主キーとする。

<div style="page-break-before:always"></div>

これを踏まえ、src\data\\**mysql_minishop_order.txt** に次の内容が記述されているので、これを用いてテーブルを作成する。

```sql
# データベースminishopを使用する
set names utf8;
use minishop;

# 注文テーブルordersの作成
drop table if exists orders;
create table orders (
    orderId     int   auto_increment   primary   key,
    orderdate   datetime
);

# 注文明細テーブルorderdetailsの作成
drop table if exists orderdetails;
create table orderdetails (
    orderId     int,
    itemId      int,
    quantity    int,
    primary key(orderId, itemId)
);
```

## 注文に関するデータベース操作を行う Order.class

データベースの基本事項を定義する「DbData.class」を継承し、注文に関するデータベース操作を行う 「Order.class」を定義する。</br>
定義するファイルは、src\classes\order.php で、この「Order.class」にカート内のすべての商品を注文内容として登録する「addOrder( )」メソッドを次の仕様で定義する。</br>

```text
アクセス修飾子： public
メソッド名： addOrder
引数： $cartItems （カート内の全ての商品を取り出した結果セット）
戻り値： なし
```

このメソッド内の具体的な処理は次の通り。

```text
1. 注文テーブルに注文日時をセットし、注文データとして登録する。
2. 登録した注文データの注文番号を取り出す。
3. 引数で受け取った$cartItemsの各商品に注文番号をセットしたデータを注文明細テーブルに登録する
```

<div style="page-break-before:always"></div>

【ヒント】</br>

- 注文テーブルにセットする注文日時は次のフォーマットを用いれば、MySQLのDATETIME型と一致する。</br>
**date("Y-m-d H:i:s")**
- 登録した注文データの注文番号は「auto_increment」で採番されるので、次の処理で取得することができる。

```php
// last_insert_id( )は、直前にinsertされたデータのauto_incrementの値を返すMySQLの関数
$sql = "select last_insert_id( ) from orders";
$stmt = $this->query( $sql,  [ ] );
$result = $stmt->fetch( );
$orderId = $result[ 0 ]; // auto_incrementで登録された注文番号(orderId)の値を取得する
```

## Cart.classに、カート内の全ての商品を削除するclearCart()メソッドを作成

受け取る引数も戻り値もなく、テーブルcartにあるすべての商品を削除する「全商品削除メソッド」を次の仕様で定義する。

```text
アクセス修飾子: public
メソッド名: clearCart
引数: なし
戻り値: なし
```

<div style="page-break-before:always"></div>

## 注文画面（order_now.php）

この画面の完成形は次のとおり。</br>
![](./images/15/order_now_display.png)

この画面と次の処理概要を参考に、src\order\order_now.php を作成する。

```text
1. Cartオブジェクトを生成する。
2. CartオブジェクトのgetItems( )メソッドを呼び出し、カート内のすべての商品を取り出す。
3. Orderオブジェクトを生成する。
4. OrderオブジェクトのaddOrder( )メソッドを呼び出し、データベースに登録する。
5. 取り出したカート内のすべての商品を画面に表示する。（金額と合計金額も表示する）
6. CartオブジェクトのclearCart( )メソッドを呼び出し、カート内の全ての商品を削除する。
```

## 動作確認

カートに複数の商品を入れ、カート内の商品画面（cart_list.php）の「注文する」リンクをクリックすると 注文画面（order_now.php）にカート内の全ての商品が表示され、データベースの注文テーブルと 注文明細テーブルにデータが登録されていることを確認する。

<div style="page-break-before:always"></div>

カート内の商品画面：cart_list.php</br>
「注文する」リンクをクリックする</br>
![](./images/15/cart_list_display.png)</br></br>

注文画面：order_now.php</br>
![](./images/15/order_now_display.png)

<div style="page-break-before:always"></div>

データベースminishopの注文テーブル（orders）と注文明細テーブル（orderdetails）には、次のようにデータが追加される。また、カートテーブル（cart）は空になっている。

- 注文テーブル: **orders**</br>
  ![](./images/15/orders.png)
  
- 注文明細テーブル: **orderdetails**</br>
  ![](./images/15/orderdetails.png)

- カートテーブル: **cart**</br>
  ![](./images/15/cart.png)

## 課題の提出と採点について

pushで提出した課題は、GitHub上で自動採点される。提出後、課題が合格しているかを確認すること。合格していない場合は修正後pushし、再提出すること。

### 課題の合格基準

以下を合格基準とする。

1. 注文処理が実行されていること
2. カート内の商品が削除されていること

<div style="page-break-before:always"></div>

### 合格確認方法

1. 本課題の[課題ページ](https://classroom.github.com/a/Tb1Lz6vU)に再度アクセスする。
2. 画面上部にある`Actions`をクリックする。</br>
![](./images/15/acions.png)
1. **一番上**の行に、緑色のチェックが入っていればOK。※その下に赤いばつ印が入っているものがあるが、それは無視する。</br>
![](./images/15/pass.png)

### エラーが出た時の対処法

自動採点がエラーになると、**一番上**の行に赤いばつ印がでる。その場合の解決策を以下に示す。

### タイムアウトになっていないかを確認する

※右端の赤枠で囲まれている箇所に処理時間がでるが、**2分以上**かかっている場合はタイムアウトとなる。
![](./images/15/timeout.png)

なお、タイムアウトの場合は、GitHub上で処理を再開すると解決できる。具体的なタイムアウト解決方法は、

  1. Actionsタブをクリック
  2. タイトルが下記のようにリンクになっているので、クリック</br>
      ![](./images/15/timeout2.png)</br>
  3. Autogradingをクリック</br>
   <img src="./images/15/timeout3.png" width="75%"></br>
   <div style="page-break-before:always"></div>

  4. 赤いばつ印が出ている箇所をクリック</br>
   <img src="./images/15/timeout4.png" width="75%"></br>
  1. `::error::Setup timed out in 120000 milliseconds`のメッセージがあればタイムアウト

  6. 右上に`Re-run jobs`(再実行)のボタンがあるので、`Re-run failed jobs`(失敗した処理だけ再実行)をクリックする。
  ![](./images/15/timeout6.png)</br>
  ![](./images/15/timeout7.png)
  7. タイムアウトにならず2分以内に処理が終了したらOK。※タイムアウトでないエラーは、次の解決策を参照。

<div style="page-break-before:always"></div>

### プログラムが正確に書かれているか確認する

プログラムが正確に書かれているかを確認すること。たとえ、ブラウザの画面でそれっぽく表示されても、自動採点なので融通がきかない。エラーが出た際は、以下を確認すること。

#### どこでエラーがでているか確認する

今回は2つの自動採点(新規ユーザー登録処理、ログイン認証処理)があるので、以下の手順で、どごでエラーが出ているか確認する。

1. Actionsタブをクリック
2. タイトルが下記のようにリンクになっているので、クリック
      ![](./images/15/timeout2.png)</br>
3. Autogradingをクリック</br>
   <img src="./images/15/timeout3.png" width="65%"></br>
4. 赤いばつ印が出ている箇所をクリック</br>
  <img src="./images/15/timeout4.png" width="65%"></br>
1. エラーがあるソースコードは、エラーメッセージが表示されるので、エラー箇所の参考にすること。</br>

#### プログラムが正確に書かれているか確認する

プログラムが正確に書かれているかを確認する。たとえ、ブラウザの画面でそれっぽく表示されても、自動採点ですので融通はききません。エラーが出た際は、サンプルコードと差異がないか確認してください。

<div style="page-break-before:always"></div>

## GitHub上での採点についてのお願い

今回、再度GitHub上での採点をするにあたりお願いがあります。それは、</br>
GitHubに課題をpushする前に、**必ずブラウザで動作確認をしてください。**　理由は下記の2つです。</br>

1. Webアプリケーションはブラウザ上で動作することが前提であるため。
2. GitHubの採点処理時間に上限があるため。</br>
以前の自動採点プログラムと比べ、処理時間の高速化には成功したものの、GitHubの合計処理時間には毎月上限があります。むやみやたらにpushすると、上限に達しかねないので、必ずブラウザ上で正常に動作することを確認してからpushしてください。**エラーの原因が特定できない場合は、お気軽に質問してください。**

