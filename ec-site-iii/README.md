# ミニショップ3

- [ミニショップ3](#ミニショップ3)
  - [事前準備](#事前準備)
  - [カート機能について](#カート機能について)
  - [カート内の商品画面の作成](#カート内の商品画面の作成)
  - [新しいテーブルcartの作成](#新しいテーブルcartの作成)
  - [カート内に関するデータベース操作を行うCart.class](#カート内に関するデータベース操作を行うcartclass)
  - [商品詳細画面（product\_detail.php）からリクエストが送られてくる「注文追加」（cart\_add.php）](#商品詳細画面product_detailphpからリクエストが送られてくる注文追加cart_addphp)
  - [カート内の商品画面（cart\_list.php）](#カート内の商品画面cart_listphp)
  - [動作確認](#動作確認)
  - [カート内の商品画面のバグ修正](#カート内の商品画面のバグ修正)
  - [動作確認](#動作確認-1)

## 事前準備

1. [こちらのページ](https://classroom.github.com/a/VVo3y7HS)から、ソースコードを`C:¥xampp¥htdocs`へcloneすること。

2. **前回の「13.ミニショップ1,2」で作成したソースコードを、今回colneしたソースコードに上書きすること。** ※ちなみに「13.ミニショップ1,2」で作成したソースコードは次のとおりである。
```text
src
├── classes
│   ├── dbdata.php  ←データベースの基本事項を定義する
│   └── product.php ←「商品」に関するProduct.class を定義する
├── footer.php        ←画面のフッターを構成する
├── header.php        ←画面のヘッダーを構成する
├── index.php         ←３つのジャンルから１つのジャンルを選択する画面
└── product
    ├── product_detail.php    ←特定の商品の詳細内容を表示する
    └── product_select.php    ←選択されたジャンルの商品一覧を表示する
```

## カート機能について

ここから、今回のミニショップで一番重要な「カート」に関する次の3つの機能を実装していく。

1. カートに商品(情報)を入れる
2. カート内の商品の注文数を変更する
3. カート内の特定の商品を削除する

そして、これらの機能がそれぞれ処理された後は、必ず処理後のカート内の商品を一覧として表示する。

cloneしたcartフォルダには次の4つのファイルが保存されている。また、classesフォルダには、Cart.classを定義する「cart.php」のファイルもある。

<div style="page-break-before:always"></div>

```text
cart 
 |-- cart_add.php // カートに商品(情報)を入れる処理を行う ・・・1
 |-- cart_change.php // カート内の商品の注文数を変更する処理を行う ・・・2
 |-- cart_delete.php // カート内の特定の商品を削除する処理を行う ・・・3
 |-- cart_list.php // カート内の商品を画面に表示する

classes
 |-- cart.php // 「カート」に関するデータベース操作を行うCart.class を定義する

```

なお、配布したこれらのファイルはいずれも何も書かれていない空ファイルである。 また、Cart.classには、次の①から④のメソッドを定義し、データベースの操作を行う。

```text
①商品(情報)をカートに入れる                  addItem( )メソッド 
②カート内の商品の注文数を変更する             changeQuantity( )メソッド 
③カート内の特定の商品を削除する               deleteItem( )メソッド 
④カート内のすべての商品を取り出す(画面表示用)   getItems( )メソッド
```

**【注意！！】今回作成する「カート内の商品」（cart_list.php）の最終完成画面を次に示すが、いきなり全て作成するのではなく、次の資料の順番で一つ一つの機能を実装しながら完成させていく。**

```text
14_ミニショップ3.pdf カート内の商品画面、カート内の商品画面のバグ修正 ← 本資料
14_ミニショップ4.pdf カート内の商品を削除
14_ミニショップ5.pdf カート内の商品の注文数を変更
```

**「カート内の商品」（cart_list.php）の最終完成画面・・・14_ミニショップ5.pdfで完成となる**

<img src="./images/14/cart_list_display.png" width="80%"></br>

<div style="page-break-before:always"></div>

## カート内の商品画面の作成

商品詳細画面（product\_detail.php）の「カートに入れる」ボタンをクリックすると、その商品がデータベースminishopのテーブルcartに登録され、カート内の商品画面(cart\_list.php)がカート内の商品を表示する。

商品詳細画面：product\_detail.php 

![](./images/14/product_detail_display.png)

カート内の商品画面：cart\_list.php

![](./images/14/cart_list_display_book.png)

<div style="page-break-before:always"></div>

複数の商品が入っている場合

<img src="./images/14/cart_list_display_three.png" width="90%"></br>

この一連の処理は次の流れとなる。

```text
1. 商品詳細画面（product_detail.php）から送られてきた商品番号と注文数を受け取る。
2. 送られてきた商品番号をキーにテーブルitemsから商品データを抽出する。
3. 抽出した商品データと受け取った注文数をテーブルcartに登録する。
4. 登録後のテーブルcart内のすべてのデータを画面に表示する。（金額と合計金額も表示する）
```

そこで、この処理の流れを次の手順で実装していく。

```text
1. 新しいテーブルcartを作成する。

2. src\classes\cart.php に Cart.classの次の２つのメソッドを定義する。
   (1) 商品番号をキーにテーブルitemsから商品データを抽出し、注文数とあわせてテーブル
      cartに登録するメソッド ・・ メソッド名：addItem( )

   (2) テーブルcart内のすべてのデータを抽出するメソッド ・・ メソッド名：getItems( )

3. 商品詳細画面（product_detail.php）から送られてくるリクエストを受け取る「注文追加」(cart_add.php) には、次の処理を記述する。
   (1) 商品詳細画面（product_detail.php）から送られてきた商品番号と注文数を受け取る。

   (2) Cartクラスのオブジェクトを生成する。

   (3) Cartオブジェクトの addItem( ) メソッドを呼び出し、テーブルcartに商品データと
      注文数を登録する。

   (4) カート内の商品を表示する「cart_list.php」を読み込む。


4. カート内の商品画面（cart_list.php）には次の処理を記述する。
   (1) Cartオブジェクトの getItems( ) メソッドを呼び出し、テーブルcart内のすべての
      データを抽出する。

   (2) 抽出したテーブルcart内のすべてのデータを画面に表示する。
      （金額と合計金額も計算して表示）
```

## 新しいテーブルcartの作成

このテーブルcartは、商品番号（項目名：ident、データ型：int）と注文数（項目名：quantity、データ型：int） だけの構造を持つ。</br>
~~そこで、**src\data\mysql\_minishop\_cart.txt** ファイルの以下の内容が記述されているので、これを元にcartテーブルを作成する。~~</br>
**7/11(火)更新**</br>
スクリプトファイルに一部誤りがありましたので、[こちらのリンク](https://classroom.google.com/c/NTYzNTU4ODExMDAx/m/NjE1OTEyNjUxNDA5/details)から、**mysql\_minishop\_cart.txt**をダウンロードし、テーブルcartを作成してください。

```sql
# データベースminishopを使用する
set names utf8;
use minishop;
	
# カートテーブルcartの作成
drop table if exists cart;
create table cart (
	ident     int   primary   key,
	quantity  int
);
```

※スクリプトファイルからデータベース環境を構築する方法を忘れた方は、「[09.データベースの作成](https://classroom.google.com/c/NTYzNTU4ODExMDAx/m/NTIyNjE1MDg2ODQ1/details)」の資料を参考にすること。

スクリプトファイルを実行すると、以下のようにminishopデータベースの中に、cartテーブルができていればOK。

![](./images/14/database_minishop.png)

![](./images/14/table_cart.png)

<div style="page-break-before:always"></div>

## カート内に関するデータベース操作を行うCart.class

データベースの基本事項を定義する「DbData.class」を継承し、カートに関するデータベース操作を行う「Cart.class」を定義する。

定義するファイルは、src\classes\cart.php で、この「Cart.class」には次の２つのメソッドを定義する。

1. 商品番号をキーにテーブルitemsから商品データを抽出し、注文数とともにテーブルcartに登録するメソッド

```text
アクセス修飾子： public
メソッド名： addItem
引数： $ident （商品番号）、$quantity（注文数）
戻り値： なし
```

2. テーブルcart内のすべてのデータを抽出するメソッド

```text
アクセス修飾子： public
メソッド名： getItems
引数： なし
戻り値： カート内のすべてのデータの結果セット
```

以上の仕様から、「Cart.class」を定義する「cart.php」を作成する。 

**ファイル：src\classes\cart.php**

![](./images/14/cart_php_code_getItems_ana.png)

<div style="page-break-before:always"></div>

【ヒント】 テーブルcartの商品番号から商品テーブルitemsの項目を取り出し、結果セットを返すためのSQL文は次の通り。

```php
$sql = "select items.ident, items.name, items.maker, items.price, cart.quantity, items.image, items.genre from cart join items on cart.ident = items.ident";
```

## 商品詳細画面（product\_detail.php）からリクエストが送られてくる「注文追加」（cart\_add.php）

このPHPファイルには、次の処理を記述する

```text
1. 商品詳細画面（product_detail.php）から送られてきた商品番号と注文数を受け取る。
2. Cartクラスのオブジェクトを生成する。
3. CartオブジェクトのaddItem( )メソッドを呼び出し、テーブルcartに商品データと注文数を登録する。
4. カート内の商品を表示する「cart_list.php」を読み込む。
```

**【注目】** この「cart\_add.php」のコードを次に示すが、今後次の2つのPHPを作成するときには、これを参考にする。

- 14\_ミニショップ4.pdfの「cart\_delete.php」(カート内の特定の商品を削除する)
- 14\_ミニショップ5.pdfの「cart\_change.php」(カート内の商品の注文数を変更する)

**ファイル：src\cart\cart_add.php**

![](./images/14/cart_add_code.png)

2行目： `require_once  __DIR__  .  '/../classes/cart.php';`

**この絶対パスの書き方(「/../」の箇所)をしっかり理解すること。** 「cart\_add.php」と「cart.php」は次に示す配置となっている。

<div style="page-break-before:always"></div>

```text
src    
 |-- cart 
 |    |-- cart_add.php
 |
 |-- classes
 |    |-- cart.php
```

この「cart\_add.php」から「cart.php」へのパスを相対パスで記述すると 1つ上の階層に上がる必要があるため「../classes/cart.php」となる。

これを絶対パスで作成する場合、まず `__DIR__` でcart\_add.phpのパスを取得するが、このときの値が `C:\xampp\htdocs\14-minishop-GitHubのユーザー名\src\cart` となる。（Windowsの場合）

そして、やはり1つ上の階層へ上がるため `/../` が必要となるので、このような書き方となる。

## カート内の商品画面（cart\_list.php）

この画面の完成形は次の通り。（複数の商品がカート内に入っている場合）

![](./images/14/cart_list_display_three.png)

この画面と次の処理概要を参考に、cart\_list.phpを作成する。

1. Cartオブジェクトの `getItems( )` メソッドを呼び出し、テーブルcart内のすべてのデータを抽出する。
2. 抽出したテーブルcart内のすべてのデータを画面に表示する。（金額と合計金額も）

<div style="page-break-before:always"></div>

**ファイル：src\cart\cart_list.php**

![](./images/14/cart_list_code_ana.png)

【ヒント】画面に表示するにあたり、各セルには次のクラス設定を行う。(13\_ミニショップ1.pdfの「**`<td>`セル内のクラス設定について**」を参照）

- 画像: `<td class="td_mini_img">` また、`<img>` には `class="mini_img"` を設定する。
- 商品名: `<td class="td_item_name">`
- メーカー・著者・アーティスト: `<td class="td_item_maker">`
- 価格: `<td class="td_right">`
- 注文数: `<td class="td_right">`
- 金額: `<td class="td_right">`
- 合計金額: `<th colspan="5">`

**※注文数と合計金額は、cart_list.php で初めてでてきた項目です。**

<div style="page-break-before:always"></div>

## 動作確認

商品詳細画面（product\_detail.php）の「カートに入れる」ボタンをクリックすると、その商品がデータベース minishop のテーブル cart に登録され、カート内の商品画面(cart\_list.php)が表示されることを確認する。

商品詳細画面：product\_detail.php

![](./images/14/product_detail_display.png)

カート内の商品画面：cart\_list.php

![](./images/14/cart_list_display_book.png)

<div style="page-break-before:always"></div>

複数の商品が入っている場合

![](./images/14/cart_list_display_three.png)

**但し、この時点ではある「バグ」が潜んでいる。 以降のページでは、バグの内容や修正方法について説明していく。**

<div style="page-break-before:always"></div>

## カート内の商品画面のバグ修正

作成したカート内の商品画面(cart_list.php)に、次のバグが発見されたので、これを修正する。

【バグ内容】

すでにカート内に入っている商品をさらに入れようとするとエラーが発生する。(ちなみに、このエラーメッセージはWindowsのみ表示されます。Macはエラーメッセージは表示されず、なんの処理も行われません。)

![](./images/14/cart_add_display_error.png)

【修正内容】</br>
すでにカート内に入っている商品をさらに入れようとした場合、注文数の追加として処理する。 （アマゾン、楽天などがこの方法で処理している） 但し、注文数は最大「10」個までとする。※追加して10個以上となる場合、今回は強制的に10個にする。

【ヒント】</br>
修正箇所は、Cartクラスの `addItem( )`メソッド。 このメソッドを次の内容に修正する。

```text
1. 今回追加する商品データが既にカート内にあるかどうかをチェックする。
2. 既にカート内にあるならば、今回の商品の注文数を加算する。
3. 但し、加算して「10」個を超えるようなら、注文数は「10」とする。
4. カート内にない商品なら、新たに注文データとして登録する。
```

<div style="page-break-before:always"></div>

## 動作確認

1. １つの商品がカートに入っている状態

![](./images/14/cart_list_display_book.png)

2. 同じ商品をさらに2個カートに入れる

![](./images/14/product_detail_display_2.png)

<div style="page-break-before:always"></div>

3. 注文数が追加され3個となった

![](./images/14/cart_add_display_3.png)

4. 同じ商品を9個カートに入れる

![](./images/14/product_detail_display_9.png)

<div style="page-break-before:always"></div>

5. 注文数は「12」とならず「10」となる

![](./images/14/cart_add_display_10.png)

**ミニショップのカートに関する機能はまだ完成ではありません。pushはしないでください。**
