# ログイン認証2

- [ログイン認証2](#ログイン認証2)
  - [事前準備](#事前準備)
  - [新規データベースおよびテーブルを作成](#新規データベースおよびテーブルを作成)
  - [プログラムの作成](#プログラムの作成)
    - [login.html](#loginhtml)
    - [register.html](#registerhtml)
    - [util.php](#utilphp)
  - [付録: `<input>` タグの `required` 属性](#付録-input-タグの-required-属性)

## 事前準備

[こちらのページ](https://classroom.github.com/a/9DxBgRkP)から、ソースコードを`C:¥xampp¥htdocs`へcloneすること。
※今回、cloneする前に、以下のようにリストから自身の名前を選択する必要があります。

![](images/12/namelist.jpeg)

```text
C:¥xampp¥htdocs
    └── 11_obj-GitHubのユーザー名
        ├── <中略>
        └── src
        |   └── classes
        |   |   ├── dbdata.php
        |   |   └── user.php
        |   └── css
        |   |   └── login.css
        |   └── script
        |   |   └── mysql_login.txt
        |   ├── login.html
        |   ├── login.php
        |   ├── register.html
        |   ├── register.php
        |   └── util.php
        └── <中略>
```

※「login.css」を利用し画面デザインを適用するため、HTMLのタグに「id」や「class」といった属性を追加している。CSSについて初めての方は、以下のサイトなどを参考に簡単な使い方などを理解しておくこと。

[【初心者向け】CSSセレクタとは？セレクタの種類や指定方法を解説！（基礎編）](https://www.asobou.co.jp/blog/web/css-selectors)

<div style="page-break-before:always"></div>

## 新規データベースおよびテーブルを作成

今回作成するデータベース名とテーブルは次のとおり。</br>
- データベース名：login 
- テーブル名：users
  
このデータベースに対するすべての操作権限を持つユーザーを「**denshi**」、そのパスワード「**kobe**」とする。

|フィールド名|データ型|その他|
| - | - | - |
|userId|varchar(8)|主キーを設定、 大文字・小文字を区別するためbinary属性を設定|
|password|varchar(12)|not null|
|userName|varchar(50)|not null|

```sql
# データベース login の作成
set names utf8;
drop database if exists login;
create database login character set utf8 collate utf8_general_ci;

# ユーザー「denshi」にパスワード「kobe」を設定し、データベース「login」に対する全ての権限を付与
grant all privileges on login.* to denshi@localhost identified by 'kobe';

# データベース login を使用
use login;

# テーブル users の作成
# mysqlでは、char、varcharカラムに対して検索条件（where）やソート（order by）時に
# 大文字小文字を区別しない　・・・　デフォルトの照合順序（utf8_general_ci）
# 区別させる場合にはテーブル作成時に binary(バイナリ)属性を設定する必要がある。
create table users(userId varchar(8) binary primary key,
                    password varchar(12) not null,
                    userName varchar(50) not null);
```

## プログラムの作成

プログラムは、次の順番で作成していく。

1. login.html・・・ユーザーID、パスワードを入力する画面
1. register.html・・・新規ユーザー登録を行うため、ユーザーID、パスワード、氏名を入力する画面
1. util.php・・・画面共通で利用するエスケープ関数をまとめたファイル
1. dbdata.php・・・データベースの基本事項を定義する
1. user.php・・・認証処理や新規ユーザの登録を行う
1. register.php・・・新規ユーザの登録処理結果を表示する画面
1. login.php・・・認証処理結果を表示する画面

<div style="page-break-before:always"></div>

### login.html

<img src="./images/12/login_html_code.png" width="95%"></br>

入力が終わったなら、必ずブラウザで以下のように正しく表示されるかを確認すること。問題がないことを確認したうえで次の「register.html」に取り掛かること。</br>
<img src="./images/12/login_html_display.png" width="65%"></br>

<div style="page-break-before:always"></div>

### register.html

<img src="./images/12/register_html_code.png" width="88%"></br>

ブラウザでの表示確認と、「ログインページへ」のリンクから遷移することを確認すること。

<img src="./images/12/register_html_display.png" width="60%"></br>

### util.php

![](./images/12/util.png)</br>

「util.php」はブラウザでの表示確認はなし。

**ログイン認証はまだ完成ではありません。まだpushはしないでください。**

## 付録: `<input>` タグの `required` 属性

htmlの `<input>`タグに `required` 属性をつけると未入力チェックができる。

<img src="./images/12/login_html_display_required.png" width="80%"></br>
<img src="./images/12/register_html_display_required.png" width="80%">
