# Webアプリケーション開発 授業概要

- [Webアプリケーション開発 授業概要](#webアプリケーション開発-授業概要)
  - [Webアプリケーションについて](#webアプリケーションについて)
    - [基本技術](#基本技術)
    - [進化の歴史](#進化の歴史)
  - [サーバーサイドプログラム](#サーバーサイドプログラム)
    - [PHP](#php)
    - [Apache](#apache)
    - [MySQL](#mysql)
    - [Docker](#docker)
    - [クライアントサイド技術の発展](#クライアントサイド技術の発展)
  - [教科書・成績評価](#教科書成績評価)
  - [課題提出のルール](#課題提出のルール)
  - [おすすめ学習サイト](#おすすめ学習サイト)
  - [プログラムの上達方法](#プログラムの上達方法)
    - [わからないことの解決ステップ　(この順番が大事！)](#わからないことの解決ステップこの順番が大事)
    - [一歩一歩の積み重ねが大事](#一歩一歩の積み重ねが大事)

## Webアプリケーションについて

- クライアント側に特別なアプリは不要でWebブラウザのみあればよい
- クライアントからリクエストを送信
- サーバはリクエストを処理し、レスポンスを返す
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/04.Web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A8%E3%81%AF.jpg?raw=true">

### 基本技術

- Webアプリケーションは、次の技術がベースとなる
  - WWW（World  Wide  Web）
  - HTML（Hyper Text Markup Language）
  - TCP/IP（Transmission Control ProtocolとInternet Protocol）
  - HTTP（Hyper Text Transfer Protocol）
  <img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/05.Web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E5%9F%BA%E6%9C%AC%E6%8A%80%E8%A1%93.jpg?raw=true">

### 進化の歴史

1. 最初はサーバーに保存されている静的コンテンツしかレスポンスできなかった
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/07.Web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E9%80%B2%E5%8C%96.jpg?raw=true">

2. その後、リクエスト時にデータを送信し、それに応じて動的コンテンツをレスポンスできるようになった
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/08.Web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E9%80%B2%E5%8C%96.jpg?raw=true">
<br>
<br>
動的コンテンツの例：Amazon(ログイン前)
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/09.Amazon(%E5%89%8D).jpeg?raw=true">
<br>
<br>
動的コンテンツの例：Amazon(ログイン後)
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/10.Amazon(%E5%BE%8C).jpeg?raw=true">

## サーバーサイドプログラム

- サーバー側で動作するプログラム
- クライアントから送られてきたデータを処理し、処理結果をクライアントに返信する
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/11.%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%B5%E3%82%A4%E3%83%89%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0.jpg?raw=true">

### PHP

- 1995年に誕生したオープンソースの汎用プログラミング言語
- コンパイル不要のスクリプト言語
- サーバーサイドで動的なウェブページを作成
- PHPの由来である“PHP:Hypertext Preprocessor”には、「Webページを構成するHTML(Hypertext)」を「事前に作り出す」という意味がある<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/14.PHP.png?raw=true">

### Apache

- 世界中でもっとも多く使われているWebサーバソフトウェア
- 大規模な商用サイトから自宅サーバまで幅広く利用されている
- 開発は、Apacheソフトウェア財団のApache Licenseの下でソースコードが公開および配布されている代表的なオープンソースソフトウェアの一つ<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/15.Apache_1.png?raw=true">

### MySQL

- オープンソースのリレーショナルデータベース管理システム(RDBMS)
- 幅広いアプリケーションで利用され、豊富なドキュメントとコミュニティサポートあり
- SQLを使用※忘れている人は2年生のテキスト「スッキリわかるSQL入門」で簡単なselect,insert,update,delete文を要復習！<br>
![](./images/logo-mysql-170x115.png)

### Docker

- アプリケーションをコンテナとしてパッケージ化し、環境差異を気にせずにどこでも実行できるオープンソースのプラットフォーム
- 本授業ではDockerを使用して、PHP、Apache、MySQLを含む開発環境を構築
- 各サービスを個別のコンテナとして実行し、それらを連携させることで、開発環境を迅速にセットアップ<br>
![](./images/docker-logo-blue.png)
  
### クライアントサイド技術の発展

- 動的コンテンツのレスポンスと同時に、プログラム(JavaScript)も送信し、クライアントサイド(ブラウザ)で実行させるようになった
- サーバーやネットワークの負荷を軽減するとともに、より高度なUI/UXを実現することができるようになった
- クライアントサイドプログラム
  - **HTML、CSS**、JavaScript

## 教科書・成績評価

- 教科書
  - Webページ上に公開
  - 紙ベースの教科書なし
- 成績評価
  - 期末試験なし
  - 課題提出のみ

## 課題提出のルール

- 提出期限を超えたものは受け取りません
- 他人のコピーが発覚した場合、評価１が確定
- 課題を提出せずに単位は取れません
  - ソフトⅢは期末試験がない科目がほとんど
  - 単位取得には日々の課題提出が必須
  - 余裕のもったスケジュール管理が必要

## おすすめ学習サイト

- [PHPマニュアル](https://www.php.net/manual/ja/index.php)
- [Progate(プロゲート)](https://prog-8.com/dashboard)
- [ドットインストール](https://dotinstall.com/)
- [paiza](https://paiza.jp/)
- [Udemy(ユーデミー)](https://www.udemy.com/ja/)

## プログラムの上達方法

- 理解するまでのステップ
  1. 聞いただけでは忘れてしまう
  2. 見ることで覚えられる
  3. やってみて初めて理解できる

### わからないことの解決ステップ　(この順番が大事！)

- わからなければ
  - 自分の頭で考える
- まだわからなければ
  - 自分で調べる  
    - マニュアル、書籍、インターネット等を如何に利用するか
  - 人に聞く
    1. 発生している問題やエラーメッセージについてまとめる
    2. どの処理までうまく動いているのかを説明する
    3. 問題が起きているコードの場所を特定する
    4. 自分自信で推測しエラー解決のために試したことをまとめる

### 一歩一歩の積み重ねが大事

- 一日サボると難易度は倍になる
- 技術の勉強は今日学んだものの上に明日は新たなものを積み上げていく<br>
 <img src="https://github.com/2024Web1/web_app_dev/blob/main/orientation/images/24.%E4%B8%80%E6%AD%A9%E4%B8%80%E6%AD%A9%E3%81%AE%E7%A9%8D%E3%81%BF%E9%87%8D%E3%81%AD.jpeg?raw=true">
