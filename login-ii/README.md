# ログイン認証②

- [ログイン認証②](#ログイン認証)
  - [事前準備](#事前準備)
    - [login.html](#loginhtml)
    - [user.php](#userphp)
  - [login.php](#loginphp)

## 事前準備

前回の「12.ログイン認証2」でcloneしたコードをそのまま利用してください。

### login.html

<img src="./images/login_html_code.png" width="95%"><br>

入力が終わったなら、必ずブラウザで以下のように正しく表示されるかを確認すること。問題がないことを確認したうえで次の「register.html」に取り掛かること。<br>
<img src="./images/login_html_display.png" width="65%"><br>

### user.php



## login.php

![](./images/login_php_code.png)

完成させた後、ブラウザで「login.html」を表示し、次のデータを入力後「ログイン」ボタンを押し、認証できることを確認する。

- ユーザーID: kobe
- パスワード: denshi

![](./images/login_html_display_input.png)
![](./images/login_php_display.png)

また、登録していないユーザーを「login.html」に入力すると、以下のように認証が失敗する。

![](./images/login_php_display_error.png)
