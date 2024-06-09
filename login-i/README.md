# ログイン認証1

- [ログイン認証1](#ログイン認証1)
  - [「ログイン認証」を行うWebアプリケーションを作成](#ログイン認証を行うwebアプリケーションを作成)
  - [詳細画面](#詳細画面)

## 「ログイン認証」を行うWebアプリケーションを作成

今回作成するWebアプリケーションでは、2つのHTML、5つのPHP、そして１つのCSSの合計8つのファイルで構成される。</br>
それぞれのファイル名と主な機能は次の通りである。

1. login.html・・・ユーザーID、パスワードを入力する画面。

1. register.html・・・新規ユーザー登録を行うため、ユーザーID、パスワード、氏名を入力する画面。

1. login.php・・・認証処理結果を表示する画面。

1. register.php・・・新規ユーザの登録処理結果を表示する画面。

1. dbdata.php・・・データベースの基本事項を定義する。

1. user.php・・・認証処理や新規ユーザの登録を行う。

1. util.php・・・画面共通で利用するエスケープ関数をまとめたファイル。

1. login.css・・・各画面のデザイン用スタイルシート。

画面遷移は次の通り。（dbdata.php, user.php, util.php, login.css は直接画面表示に関係ないので、下図では省略）

![](./images/12/page_transition.png)

<div style="page-break-before:always"></div>

## 詳細画面

1. 新規登録画面（register.html）</br>
新規ユーザーが、「ユーザーID」、「パスワード」、「名前」をデータベースに登録する入力フォームを用意する。</br>
![](./images/12/register_html_display.png)

1. 登録結果画面（register.php）</br>
ユーザー登録が完了した場合、登録した「ユーザーID」、「パスワード」、「名前」を画面に表示する。</br>
※実際のシステムでは、パスワードを見せることはせずに「\*\*\*\*\*」といった形で表示する ここではあえて見える形で表示している</br>
![](./images/12/register_php_display.png)</br></br>
登録済みのユーザーIDを使った場合にエラーとし、入力されたユーザーIDを画面に表示する。</br>
![](./images/12/register_php_display_error.png)

1. ログイン画面（login.html）</br>
新規登録完了後、登録したユーザーIDとパスワードを入力する。</br>
![](./images/12/login_html_display.png)

<div style="page-break-before:always"></div>

1. 認証結果画面(login.php)</br>
入力したユーザーIDとパスワードが、データベースに登録した内容と一致した場合、ログインを認め 「こんにちは、○○○○ さん！」とログインしたユーザー名を表示する。</br>
![](./images/12/login_php_display.png)</br></br>
登録されていないユーザーIDやパスワードでログインしようとした場合には、次のエラー画面を表示する。</br>
![](./images/12/login_php_display_error.png)</br>
