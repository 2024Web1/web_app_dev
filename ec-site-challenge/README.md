﻿# ミニショップチャレンジ問題:ログイン機能を追加しよう

- [ミニショップチャレンジ問題:ログイン機能を追加しよう](#ミニショップチャレンジ問題ログイン機能を追加しよう)
  - [はじめに](#はじめに)
  - [事前準備](#事前準備)
  - [本章から追加されたテーブルについて](#本章から追加されたテーブルについて)
  - [仕様について](#仕様について)
  - [留意事項](#留意事項)
  - [ヒント](#ヒント)
  - [動作確認](#動作確認)
  - [参考資料](#参考資料)

## はじめに

本章は、ミニショップを最後まで作成した方に向けたチャレンジ問題です。
評価対象外ですが、ログイン機能を追加した、より実用的なWebアプリケーションを作成する足掛かりとして、ぜひ挑戦してみてください。
なお、チャレンジ問題ですので、仕様などあえて**ざっくりとした記載となっています**ので、自分で考えながらプログラムを作成してください。

## 事前準備

1. [こちらのページ](https://classroom.github.com/a/Uov7jhFs)から、ソースコードを`C:¥web_app_dev`へcloneしてください。

2. **「仕様書⑥」までで作成した`public`ディレクトリ内のソースコードを、今回colneした`public`ディレクトリにコピーしてください。**
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
│   ├── order.php
│   └── product.php
├── css
│   ├── login.css ←ログイン認証の章で作成したCSSを一応用意していますが、不要であれば削除してください。
│   └── minishop.css
├── images
├── index.php
└── order
│   └── order_now.php
└── product
    ├── product_detail.php
    └── product_select.php
```

## 本章から追加されたテーブルについて

新たに**ユーザー認証を行うためのテーブルusers**が追加されます。
また、ユーザー管理を行うために、cart, ordersのテーブルに**userIdカラムが追加**されます。
※itemsテーブルに変更はありません。

**テーブル名：users**

| カラム名 | データ型 | 制約 | 備考 |
| - | - | - | - |
|userId|int型|主キー、auto_increment|ユーザー番号|
|password|varchar(12)||パスワード|
|userName|varchar(50)||ユーザー名|

**テーブル名：cart**

| カラム名 | データ型 | 制約 | 備考 |
| - | - | - | - |
|userId|int型|主キー|ユーザー番号|
|ident|int型|主キー|商品番号|
|quantity|int型||数量|

※uesrIdとidentの複合主キーとなります。

**テーブル名：orders**

| カラム名 | データ型 | 制約 | 備考 |
| - | - | - | - |
|orderId|int型|主キー、auto_increment|注文番号|
|userId|int型||ユーザー番号|
|orderdate|datetime型||注文日時|

## 仕様について

以下を満たすようにプログラムを作成してください。

1. ログイン画面を作成すること
2. 新規ユーザー登録画面を作成すること
3. ユーザーごとにカート情報を管理すること
4. ユーザーごとに注文情報を管理すること
5. ログアウトができること
6. ログインしていなければミニショップの画面にアクセスしようとしても、ログイン画面にリダイレクトされること

なお、ログイン機能に関するクラス`User`は、classes/user.phpに、ログイン画面や新規登録画面など

## 留意事項

いちからログイン機能を実装しても良いですが、ログイン認証の章で作成したログイン機能を流用すると効率よく開発できます。

## ヒント

1. ログイン認証の章で登場したログイン後の画面「welcome.php」が、ミニショップの画面になると考えると良い
2. ユーザーごとにカートや注文情報を管理するには、SQL文の条件にuserIdを追加すると良い

## 動作確認

以下の手順で動作確認を行ってください。

1. ユーザーを新規登録する
2. 登録したユーザーでログインする
3. 商品をカートに追加する
4. 注文する
5. ログアウトする
6. 商品一覧画面(product_select.php)にアクセスしようとすると、ログイン画面にリダイレクトされる
7. 別のユーザーで1~3を繰り返す
8. 「7.」とは別のユーザーで1~3を繰り返し、カートの中身が他のユーザーとは別であることを確認する
9. 注文する

## 参考資料

1. ミニショップの仕様書①〜⑥
2. ログイン認証の章

本章は評価対象外です。
解答例は9月末に公開予定です。

