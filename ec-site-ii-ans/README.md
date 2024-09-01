# ミニショップ②解答(仕様書②:商品詳細画面)

- [ミニショップ②解答(仕様書②:商品詳細画面)](#ミニショップ解答仕様書商品詳細画面)
  - [解答資料について](#解答資料について)
  - [product/product\_detail.php](#productproduct_detailphp)

## 解答資料について

穴抜き部分の行のみ、解答を記載しています。
**コード全てを記載しているわけではない**ので、解答例と元資料を参考に、自分でコードを完成させてください。

## product/product_detail.php

```text
<?php
// ジャンル別商品一覧から送られてきた商品番号を受け取る(穴埋め)
$ident = $_GET['ident'];
// product.phpを読み込む(穴埋め)
require_once __DIR__ . '/../classes/product.php';
// Productオブジェクトを生成する(穴埋め)
$product = new Product();
// 送られてきた商品番号で商品を抽出するメソッドを呼び出し、抽出された商品データを受け取る(穴埋め)
$item = $product->getItem($ident);

<!-- 商品番号をhiddenで送る(穴埋め) -->
<input type="hidden" name="ident" value="<?= $item['ident'] ?>">

<!-- 商品名を出力する(穴埋め) -->
<td><?= $item['name'] ?></td>

<!-- imgタグを使い、商品画像を出力する(穴埋め) -->
<img class="detail_img" src="../images/<?= $item['image'] ?>">

<!-- メーカー・著者・アーティストを出力する(穴埋め) -->
<td><?= $item['maker'] ?></td>

!-- 価格を出力する、価格はカンマ(,)区切りにする必要がある。-->
<!--「product_select.php」で同様のやり方をしているので参考にすること(穴埋め) -->
<td>&yen;<?= number_format($item['price']) ?></td>
```
