# ミニショップ③、④、⑤解答(仕様書③、④、⑤:カート機能)

- [ミニショップ③、④、⑤解答(仕様書③、④、⑤:カート機能)](#ミニショップ解答仕様書カート機能)
  - [解答資料について](#解答資料について)
  - [ミニショップ③](#ミニショップ)
    - [classes/cart.php](#classescartphp)
    - [cart/cart\_list.php](#cartcart_listphp)
  - [ミニショップ④](#ミニショップ-1)
    - [cart/cart\_list.php](#cartcart_listphp-1)
    - [cart/cart\_delete.php](#cartcart_deletephp)
    - [classes/cart.php](#classescartphp-1)
  - [ミニショップ⑤](#ミニショップ-2)
    - [cart/cart\_list.php](#cartcart_listphp-2)
    - [cart/cart\_change.php](#cartcart_changephp)
    - [classes/cart.php](#classescartphp-2)

## 解答資料について

同じPHPファイルの解答を複数回記載していますが、それはミニショップ③から⑤に従って、段階的に機能を追加している仕様書に対応するためです。
**また、コードの一部のみ記載しているものと、全文記載しているものとに分かれます。それぞれPHPファイル名の下にどちらかを記載しておりますので、間違えないように気をつけてください。**

## ミニショップ③

### classes/cart.php

穴抜きになっている部分のコードのみ記載しています。

```text
class Cart extends DbData
{
  // 商品をカートに入れるメソッド
  // 【ヒント】商品番号($ident)と注文数($quantity)をテーブルcartに登録する処理①、②を記述する
  public function addItem($ident, $quantity)
  {
    // 追加しようとしている商品データが既にカート内にあるかどうかをチェックする
    $sql = "SELECT * FROM cart WHERE ident = ?";
    $stmt = $this->query($sql, [$ident]);
    $cart_item = $stmt->fetch();
    // 既にカート内にあった場合、その商品の注文数を加算する
    if ($cart_item) {
      $new_quantity = $quantity + $cart_item['quantity'];
      // ただし、加算した注文数が「10」個を超えた場合、注文数を「10」とする
      if ($new_quantity > 10) {
        $new_quantity = 10;
      }
      $sql = "UPDATE cart SET quantity = ? WHERE ident = ?";
      $this->exec($sql, [$new_quantity, $ident]);
      // カート内になかった場合、新たにカートに追加する
    } else {
      // ① SQL文を定義する
      $sql = "INSERT INTO cart VALUES( ?,  ? )";
      // ② 実行する
      $this->exec($sql,  [$ident, $quantity]);
  }

  // カート内のすべてのデータを取り出すメソッド
  // 【ヒント】テーブルcart内のすべてのデータを抽出し、商品テーブルitemsの結果セットを返す処理①〜④を記述する
  public function getItems()
  {
    // ① SQL文を定義する
    $sql = "SELECT items.ident, items.name, items.maker, items.price, cart.quantity, items.image, items.genre FROM cart JOIN items ON cart.ident = items.ident";
    // ② 実行する
    $stmt = $this->query($sql, []);
    // ③ 結果セットを取り出す
    $items = $stmt->fetchAll();
    // ④ 結果セットを戻り値として返す
    return $items;
  }
}
```

### cart/cart_list.php

```text
<?php
  // CartオブジェクトのgetItemsメソッドを呼び出し、テーブルcart内のすべてのデータを抽出す
  // ※なお、cart_add.phpで既にCartオブジェクトを生成しているため、Cartオブジェクトの再生成は不要。
  $cartItems = $cart->getItems();
  $total = 0;
  foreach ($cartItems as $item) {
  ?>
    <!-- 抽出したテーブルcart内のすべてのデータを画面に表示する。 -->
    <!-- 商品一覧の表示に関しては、ジャンル別商品一覧画面(product_select.php)を -->
    <!-- 参考にすると作りやすい。ただし、注文数欄、金額欄、合計金額欄の追加を忘れないこと。 -->
    <tr>
      <td class="td_mini_img"><img class="mini_img" src="../images/<?= $item['image'] ?>"></td>
      <td class="td_item_name"><?= $item['name'] ?></td>
      <td class="td_item_maker"><?= $item['maker'] ?></td>
      <td class="td_right">&yen;<?= number_format($item['price']) ?></td>
      <td class="td_right"><= $item['quantity'] ?></td>
      <td class="td_right">&yen;<?= number_format($item['price'] * $item['quantity']) ?></td>
    </tr>
  <?php
    $total += $item['price'] * $item['quantity'];
  }
  ?>
  <tr>
    <th colspan="5">合計金額</th>
    <td class="td_right">&yen;<?= number_format($total); ?></td>
  </tr>
</table>
```

## ミニショップ④

### cart/cart_list.php

資料が穴埋め形式ではなく、また修正箇所も限定的であり、今までのような差分のみ記載する形式が難しいため、全文記載しています。

```text
<!DOCTYPE html>
<html lang="ja">

<head>
	<meta charset="UTF-8">
	<title>ショッピングサイト</title>
	<link rel="stylesheet" href="../css/minishop.css">
</head>

<body>
	<?php
	// カート内の商品の有無を確認する
	$cartItems = $cart->getItems(); // 仕様書④からここに移動
	//var_dump($cartItems);
	// 商品がひとつもない場合
	if (empty($cartItems)) {
		// 「お客様のショッピングカートに商品はありません。」というメッセージを表示する
		echo '<h4>お客様のショッピングカートに商品はありません。</h4>';
		// 「注文する」リンクを削除する
		echo '<a href="../index.php">ジャンル選択に戻る</a>';
		// 商品がある場合、一覧表を表示する
	} else {
	?>
		<h3>カートの商品</h3>
		<table>
			<tr>
				<th>&nbsp;</th>
				<th>商品名</th>
				<th>メーカー・著者<br>アーティスト</th>
				<th>価格</th>
				<th>注文数</th>
				<th>金額</th>
				<!-- 一覧表の見出し行に「削除」欄を追加 -->
				<th>削除</th>
			</tr>

			<?php
			// CartオブジェクトのgetItemsメソッドを呼び出し、テーブルcart内のすべてのデータを抽出す
			// ※なお、cart_add.phpで既にCartオブジェクトを生成しているため、Cartオブジェクトの再生成は不要。										
			// $cartItems = $cart->getItems(); ← 仕様書④からif分の条件とするため移動
			$total = 0;
			foreach ($cartItems as $item) {
			?>
				<!-- 抽出したテーブルcart内のすべてのデータを画面に表示する。 -->
				<!-- 商品一覧の表示に関しては、ジャンル別商品一覧画面(product_select.php)を -->
				<!-- 参考にすると作りやすい。ただし、注文数欄、金額欄、合計金額欄の追加を忘れないこと。 -->
				<tr>
					<td class="td_mini_img"><img class="mini_img" src="../images/<?= $item['image'] ?>"></td>
					<td class="td_item_name"><?= $item['name'] ?></td>
					<td class="td_item_maker"><?= $item['maker'] ?></td>
					<td class="td_right">&yen;<?= number_format($item['price']) ?></td>
					<td class="td_right"><= $item['quantity'] ?></td>
					<td class="td_right">&yen;<?= number_format($item['price'] * $item['quantity']) ?></td>
					<td>
						<!-- 各商品の「削除」欄には「削除」ボタンを追加する -->
						<!-- 「削除」ボタンは、リンクではなく`<form>`タグの中に`<input type="submit>`で実装すること -->
						<form method="POST" action="cart_delete.php">
							<!-- 「削除」ボタンをクリックするとその商品の「商品番号」を商品削除機能(cart_delete.php)に送信する -->
							<input type="hidden" name="ident" value="<?= $item['ident'] ?>">
							<input type="submit" value="削除">
						</form>
					</td>
				</tr>
			<?php
				$total += $item['price'] * $item['quantity'];
			}
			?>
			<tr>
				<th colspan="5">合計金額</th>
				<td class="td_right">&yen;<?= number_format($total); ?></td>
				<!-- 仕様書④から追加する表の一番右下の空白セル -->
				<td>&nbsp;</td>
			</tr>
		</table>
		<br>
		<!-- リンクは「ジャンル選択に戻る」と「注文する」の２つを表示する(「注文する」のリンク先は、order/order_now.php のファイル) -->
		<a href="../index.php">ジャンル選択に戻る</a>&nbsp;&nbsp;<a href="../order/order_now.php">注文する</a>
	<?php
	}
	?>
</body>

</html>
```

### cart/cart_delete.php

全文記載しています。

```text
<?php
// カート内の商品（cart_list.php）から送られてきた「商品番号」を受け取る			
$ident = $_POST['ident'];

// クラス`Cart`のオブジェクトを生成し、受け取った「商品番号」を引数として、商品削除メソッド(`deleteItem`)を呼び出す		
require_once __DIR__ . '/../classes/cart.php';
$cart = new Cart();
$cart->deleteItem($ident);

// カート内の商品画面(cart_list.php)を読み込む			
require_once __DIR__ . '/cart_list.php';
```

### classes/cart.php

Cartクラスに追加するメソッドのみ記載しています。

```text
// 引数として受け取った「商品番号」をキーに、テーブルcartから該当商品を削除する「商品削除メソッド」
public function deleteItem($ident)
{
  $sql = "DELETE FROM cart WHERE ident = ?";
  $this->exec($sql, [$ident]);
}
```

## ミニショップ⑤

### cart/cart_list.php

資料が穴埋め形式ではなく、また修正箇所も限定的であり、今までのような差分のみ記載する形式が難しいため、全文記載します。

```text
<!DOCTYPE html>
<html lang="ja">

<head>
	<meta charset="UTF-8">
	<title>ショッピングサイト</title>
	<link rel="stylesheet" href="../css/minishop.css">
</head>

<body>
	<?php
	// カート内の商品の有無を確認する
	$cartItems = $cart->getItems(); // 仕様書④からここに移動
	//var_dump($cartItems);
	// 商品がひとつもない場合
	if (empty($cartItems)) {
		// 「お客様のショッピングカートに商品はありません。」というメッセージを表示する
		echo '<h4>お客様のショッピングカートに商品はありません。</h4>';
		// 「注文する」リンクを削除する
		echo '<a href="../index.php">ジャンル選択に戻る</a>';
		// 商品がある場合、一覧表を表示する
	} else {
	?>
		<h3>カートの商品</h3>
		<table>
			<tr>
				<th>&nbsp;</th>
				<th>商品名</th>
				<th>メーカー・著者<br>アーティスト</th>
				<th>価格</th>
				<th>注文数</th>
				<th>金額</th>
				<!-- 一覧表の見出し行に「削除」欄を追加 -->
				<th>削除</th>
			</tr>

			<?php
			// CartオブジェクトのgetItemsメソッドを呼び出し、テーブルcart内のすべてのデータを抽出す
			// ※なお、cart_add.phpで既にCartオブジェクトを生成しているため、Cartオブジェクトの再生成は不要。										
			// $cartItems = $cart->getItems(); ← 仕様書④からif分の条件とするため移動
			$total = 0;
			foreach ($cartItems as $item) {
			?>
				<!-- 抽出したテーブルcart内のすべてのデータを画面に表示する。 -->
				<!-- 商品一覧の表示に関しては、ジャンル別商品一覧画面(product_select.php)を -->
				<!-- 参考にすると作りやすい。ただし、注文数欄、金額欄、合計金額欄の追加を忘れないこと。 -->
				<tr>
					<td class="td_mini_img"><img class="mini_img" src="../images/<?= $item['image'] ?>"></td>
					<td class="td_item_name"><?= $item['name'] ?></td>
					<td class="td_item_maker"><?= $item['maker'] ?></td>
					<td class="td_right">&yen;<?= number_format($item['price']) ?></td>
					<!-- <td class="td_right"><= $item['quantity'] ?></td> 仕様書④までの元々の注文数のコードは削除 -->

					<td>
						<!-- 「変更」ボタンを追加し、クリックすると「注文数変更」(cart_change.php)に商品番号と変更後の注文数を送信する -->
						<form method="POST" action="cart_change.php">
							<select name="quantity">
								<?php
								// テーブルcartに登録されている注文数を最大値「10」までのプルダウンメニューを使用し、現在の注文数を表示する
								for ($i = 1; $i <= 10; $i++) {
									echo '<option value="' . $i . '"';
									// 現在の注文数を選択状態にする
									if ($i == $item['quantity']) {
										echo ' selected>';
									} else {
										echo '>';
									}
									echo $i . '</option>';
								}
								?>
							</select>
							<input type="hidden" name="ident" value="<?= $item['ident'] ?>">
							<!-- 「変更」ボタンは、リンクではなく`<form>`タグの中に`<input type="submit>`で実装すること -->
							&nbsp;<input type="submit" value="変更">
						</form>
					</td>
					<td class="td_right">&yen;<?= number_format($item['price'] * $item['quantity']) ?></td>
					<td>
						<!-- 各商品の「削除」欄には「削除」ボタンを追加する -->
						<!-- 「削除」ボタンは、リンクではなく`<form>`タグの中に`<input type="submit>`で実装すること -->
						<form method="POST" action="cart_delete.php">
							<!-- 「削除」ボタンをクリックするとその商品の「商品番号」を商品削除機能(cart_delete.php)に送信する -->
							<input type="hidden" name="ident" value="<?= $item['ident'] ?>">
							<input type="submit" value="削除">
						</form>
					</td>
				</tr>
			<?php
				$total += $item['price'] * $item['quantity'];
			}
			?>
			<tr>
				<th colspan="5">合計金額</th>
				<td class="td_right">&yen;<?= number_format($total); ?></td>
				<!-- 仕様書④から追加する表の一番右下の空白セル -->
				<td>&nbsp;</td>
			</tr>
		</table>
		<br>
		<!-- リンクは「ジャンル選択に戻る」と「注文する」の２つを表示する(「注文する」のリンク先は、order/order_now.php のファイル) -->
		<a href="../index.php">ジャンル選択に戻る</a>&nbsp;&nbsp;<a href="../order/order_now.php">注文する</a>
	<?php
	}
	?>
</body>

</html>
```

### cart/cart_change.php

全文記載しています。

```text
<?php
// カート内の商品画面(cart_list.php)から送られてきた「商品番号」と「注文数」を受け取る					
$ident = $_POST['ident'];
$quantity = $_POST['quantity'];

// クラス`Cart`のオブジェクトを生成し、受け取った「商品番号」と「注文数」を引数に注文数変更メソッドを呼び出す					
require_once __DIR__ . '/../classes/cart.php';
$cart = new Cart();
$cart->changeQuantity($ident, $quantity);

// カート内の商品画面(cart_list.php)を読み込む					
require_once __DIR__ . '/cart_list.php';
```

### classes/cart.php

Cartクラスに追加するメソッドのみ記載しています。

```text
// 引数として受け取った「商品番号」をキーにテーブルcartの該当商品の「注文数」を変更する注文数変更メソッド
public function changeQuantity($ident, $quantity)
{
  $sql = "UPDATE cart SET quantity = ? WHERE ident = ?";
  $this->exec($sql, [$quantity, $ident]);
}
```
