# Git、GitHub

- [Git、GitHub](#gitgithub)
  - [事前準備](#事前準備)
    - [Visual Studio Codeのインストール](#visual-studio-codeのインストール)
    - [Winget](#winget)
      - [Wingetのインストール](#wingetのインストール)
    - [Git](#git)
      - [Gitのインストール](#gitのインストール)
      - [インストールしたけどgitコマンドが見つからない場合](#インストールしたけどgitコマンドが見つからない場合)
    - [GitHub](#github)
    - [GitHubアカウント作成(※既にGitHubアカウントを作成済の方は不要)](#githubアカウント作成既にgithubアカウントを作成済の方は不要)
  - [課題へのアクセス、受諾](#課題へのアクセス受諾)
    - [Gitの下準備](#gitの下準備)
    - [VSCode(Visual Studio Code)でのコード取得](#vscodevisual-studio-codeでのコード取得)
      - [プラグインGit Graphインストール](#プラグインgit-graphインストール)
      - [cloneできなかった場合](#cloneできなかった場合)
  - [作成と提出](#作成と提出)
    - [テキストファイルの追加(add)](#テキストファイルの追加add)
    - [テキストファイルをコミット(commit)する](#テキストファイルをコミットcommitする)
    - [テキストファイルをプッシュ(push)する](#テキストファイルをプッシュpushする)
  - [付録(Git関連の用語説明)](#付録git関連の用語説明)

## 事前準備

### Visual Studio Codeのインストール

VSCodeは拡張性の高いテキストエディタで、多数のプラグインが利用可能です。
プラグインによりコーディング支援やデバッグ、バージョン管理が容易になり、導入により効率的な開発が可能になります。

1. インストールファイルのダウンロード<br>
Visual Studio Code(以下、「VSCode」と呼ぶ)のWebサイトにアクセスし、ダウンロードする。 2022年4月11日時点では、`VSCodeUserSetup-x64-1.66.1.exe`がダウンロードできる。インストールサイトは[こちら。](https://code.visualstudio.com/Download)<br>
![](./images/Aspose.Words.a262145e-b3c2-4e51-a607-826ad36f58eb.001.png)

1. インストーラの実行<br>
ダウンロードしたファイルをダブルクリックし、インストーラを起動する。

1. セットアップ開始・インストール先の指定<br>
「同意する」を選択し、「次へ」をクリックする。 デフォルトのまま「次へ」をクリックする。<br>
![](./images/Aspose.Words.a262145e-b3c2-4e51-a607-826ad36f58eb.002.png)

1. スタートメニューフォルダーの指定・追加タスクの選択<br>
デフォルトのまま「次へ」をクリックする 次の２つのみチェックを入れ、「次へ」。<br>
![](./images/Aspose.Words.a262145e-b3c2-4e51-a607-826ad36f58eb.003.png)

1. インストール開始<br>
「インストール」をクリックし、インストールを開始する。<br>
![](./images/Aspose.Words.a262145e-b3c2-4e51-a607-826ad36f58eb.004.png)

1. VSCodeを起動する<br>
「Visual Studio Codeを実行する」にチェックを入れ、「完了」をクリックして、起動させる。<br>
![](./images/Aspose.Words.a262145e-b3c2-4e51-a607-826ad36f58eb.005.png)

### Winget

WingetはWindows 10/11⽤パッケージ管理ツールです。 Wingetを使うことでアプリケーションの検索、インストール、アップデートが簡単にできます。<br>

#### Wingetのインストール

既にインストールされている場合もあるので、先に確認しましょう。 PowerShellもしくはコマンドプロンプトから `winget -v` コマンドを実行し、バージョン(例：`v1.X.XXXXX`) が表示されれば既にインストールされています。<br>

Wingetを利⽤する場合、Microsoft Storeからインストールを⾏う必要があります。Microsoft Storeで"Winget"と検索すると「アプリ インストーラ」※という名前のアプリが⾒つかります。「アプリ インストーラ」のインストールを⾏ってください。<br>
※「アプリ インストーラ」が正式名称です。⼀般的にWingetと呼ばれていることが多いので、本資料でもWingetと呼びます。 <br><br>
正しくインストールされたか確認したいときは、PowerShellもしくはコマンドプロンプトから、再度 `winget -v` で確認し、バージョン(例：`v1.X.XXXXX`)が表示されればOKです。<br><br>

<div style="page-break-before:always"></div>

### Git

バージョン管理ツールの一種です。 Gitの詳細については、別途セミナーで学びます。ですので、今回は課題の提出方法として利用することに注力してください。<br>

#### Gitのインストール

※Macはデフォルトでインストールされています。<br>
PowerShellもしくはコマンドプロンプトから下記コマンドを実行する。<br>

```shell
winget install --id Git.Git
```

正しくインストールされたか確認したいときは、`git --version`でバージョン(例:`git version 2.XX.X`)が表示されればOKです。<br>

#### インストールしたけどgitコマンドが見つからない場合

`git --version`コマンドを実行した際、`‘git’ is not recognized as an internal or external command`とエラーになることがあります。環境変数がなぜか設定されていない可能性があるので、下記画像を参照に設定してください。<br>

1. Windowsキー → `environ` と入力 → システム環境変数の編集<br>
<img src="./images/git_pass1.jpg" width="50%">

2. 環境変数を押す<br>
<img src="./images/git_pass2.jpg" width="40%">

3. 下部のシステム環境変数の`Path`の行を選択し、編集を押す。<br>
<img src="./images/environment.png" width="40%">

4. 新規をクリックし、`C:¥Program Files¥Git¥cmd`を追加する。赤枠内のように設定できればOK。<br>
<img src="./images/git_pass3.jpg" width="50%">

5. PowerShellもしくはコマンドプロンプトを再起動し、`git --version`を実行し、バージョンが表示されればOK。

<div style="page-break-before:always"></div>

### GitHub

GitHub はGitリポジトリのホスティングサービスです。

インターネット上にリモートリポジトリを用意することで、離れた場所にいるメンバー同士で開発を行ったり、ソースコードを世界中に公開することができます。

### GitHubアカウント作成(※既にGitHubアカウントを作成済の方は不要)

1. GitHubのサイトにアクセスしてください。[GitHubのサイトはこちら](https://github.co.jp/)<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.003.png?raw=true">
2. 画面中央付近にある、緑色のGithub に登録するボタンをクリックしてください。
3. ここから先はUIが英語になります。ユーザー名、メールアドレス、パスワードを入力してください。
4. Verify your accountから指示に従ってアカウント認証を行ってください。<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.004.png?raw=true">
5. 認証が完了したらCreateaccountボタンをクリックします。
6. 登録したメールアドレス宛にEnter codeが送られてきますので、そちらを入力します。
7. アンケートに答えてください。(2回あります。)※アンケートが表示されない場合もあるので、その時はこの作業は無視してください。<br><img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.005.png?raw=true"><br>
8. プランの選択画面になりますので、Free(左側)を 選択しましょう。プランはあとから変更できます。※プランの選択画面が表示されない場合もあるので、その時はこの作業は無視してください。<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.007.jpeg?raw=true">

1. 登録完了です。

<div style="page-break-before:always"></div>

## 課題へのアクセス、受諾

課題ページは[こちら](https://classroom.github.com/a/QM3UUgtQ)

1. GitHubアカウントでログインしてください
2. GitHub Classroomには既に皆さんの名前を登録しています、自分の名前を選んでクリックして進めてください<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/join_the_classroom.png?raw=true"><br><br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.009.png?raw=true"><br><br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/accept_the_assignment.png?raw=true">

1. 招待の受け入れをすると、課題リポジトリのリンクが表示されます。リンクが表示されない場合は、ブラウザをリロードしてください。<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/repository_link.png?raw=true">

1. リポジトリリンク(上記の水色背景の行)をクリックすると、課題用に作成されたリポジトリにアクセスできます。
2. clone(取得)用のURLは、緑のボタン(code)から確認できます。httpsを選び、コピー用のボタンでクリップボードに一度取り込んでください。<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.013.jpeg?raw=true">

### Gitの下準備

初めてGitを使う方は、commit(登録)の際に使う名前とメールアドレスを登録しましょう。 PowerShellもしくはコマンドプロンプトから下記コマンドを実行してください。**{}の入力はいりません。自分の名前、メールアドレスは半角英数字で！**

```shell
git config --global user.name {自分の名前}
git config --global user.email {メールアドレス}
```

確認には、下記コマンドを実行します。出力に`user.name`と`user.email`の項目があるので、設定したとおりになっていればOKです。

```shell
git config --list
```

<br>

### VSCode(Visual Studio Code)でのコード取得

#### プラグインGit Graphインストール

1. 本授業のclone用フォルダ `C:¥web_app_dev` を作成してください。
2. VSCodeにて、Ctrl+Shift+Xを同時に押します。
3. "Search Extentions in Marketplace"の欄に"Git Graph"と入力します。
4. "Git Graph"のInstallボタンを押します
5. インストールが完了し、サイドバーにGit Graphのアイコン<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.016.png?raw=true">が追加されていることを確認します。
6. Git Graphのアイコンをクリックし、リポジトリのクローンを押します。(もしくは、`Ctrl+Shift+P`を押し、フォームに`git: clone`と入力し、`Git:クローン`を押すのでも可)先ほどコピーしたリポジトリのURLを貼り付け、Enterを押してください。
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.017.png?raw=true"><br>

7. フォルダの選択画面になるので、`C:¥web_app_dev` フォルダを選択してください。
8. 認証を求められるので、ブラウザでアカウントを入れて認証してください。(※求められなければ無視してください。)<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.018.jpeg?raw=true">

9. 認証に成功すれば、`C:¥web_app_dev` 直下にコードがcloneできています。<br><br>

#### cloneできなかった場合

現象と解決策は下記のいずれかと考えられます。

1. `repository not found`とエラーが出る。<br>
  過去に別のGitHubアカウントを作成し、Gitを利用した経験がある方は、`repository not found`のエラーでcloneできない場合があります。その場合は、下記サンプルのように、cloneするリポジトリのURLに`ユーザー名@`を追記し、再度cloneをしてください。※このユーザー名はアカウント作成時に登録したユーザー名です。<br>
    ```
    https://ユーザー名@github.com/〜.git
    ```

1. cloneが終わらない。<br>
  エラーは出ないが、cloneがいつまで経っても終了しない場合があります。実際は、別ウインドウ・ブラウザで、GitHubアカウントの認証待ちの状態になっていることがあるので、認証を済ませてください。

1. `user.name`と`user.email`が設定できていない。<br>
  [Gitの下準備](#gitの下準備)に戻って、設定し直してください。

<div style="page-break-before:always"></div>

## 作成と提出

### テキストファイルの追加(add)

1. 課題として提出するファイルをVSCodeで開きます。VSCodeのメニューから「ファイル->フォルダーを開く」を選択し、`C:¥web_app_dev¥01-git...`を選択します。
2. `kadai.txt`を開きます。「A.」の横に好きな食べ物を入力し、保存してください。
3. VSCodeサイドバーのGit Graphのアイコン<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.016.png?raw=true">を押します。
4. 変更の欄に`kadai.txt`が表示されていることを確認し、+ボタンを押します。<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.019.png?raw=true">

1. `kadai.txt`がステージされている変更に移動していれば、addは成功です。

<div style="page-break-before:always"></div>

### テキストファイルをコミット(commit)する

commitを行うためには、変更理由を記録する必要があります。最初に変更理由の記録からはじめます。

1. メッセージの欄に変更理由を入力します。ここでは「好きな食べ物を入力」とします。※メッセージを入力しないとcommitできないので、今後も注意！<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.020.png?raw=true">

2. ✔のボタンを押すとcommitは完了です。<br><br>

### テキストファイルをプッシュ(push)する

あとは課題を提出するのみです。

1. 変更の同期ボタンを押します。<br><img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.022.png?raw=true">

1. ブラウザで、再度課題のリンクにアクセスすると(cloneで使ったURLでも良い)、編集内容が反映されているのがわかります。(kadai.txtを押して、中身も変わっているか確認しましょう。)<br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.021.png?raw=true">
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/Aspose.Words.aedafcf0-3819-4263-af12-50337a38362b.023.png?raw=true">

1. 提出完了です。今後の課題は、このようにGitで提出してもらいますので、よろしくお願いします。<br><br>

<div style="page-break-before:always"></div>

## 付録(Git関連の用語説明)

- **リポジトリ（Repository）**: Gitで管理されるプロジェクトの全ての変更履歴やファイルを保存するデータベースのようなものです。

- **ローカルリポジトリ（Local Repository）**: プロジェクトを自分のローカル環境に複製したリポジトリのことです。自分のPCやサーバーに保存されており、ローカルで変更を加え、コミットやプッシュを行うことができます。

- **リモートリポジトリ（Remote Repository）**: プロジェクトを共有するために、複数の開発者がアクセスできるリポジトリのことです。一般的には、オンラインのGitリポジトリホスティングサービス（例: GitHub, GitLab, Bitbucketなど）に保存されています。
  
- **ステージングエリア(staging area)**: リポジトリにコミットする前に変更を一時的に準備する場所です。ステージングエリアを使用することで、コミットする前に変更の内容を選択的に追加 (ステージ) し、コミットすることができます。

- **クローン(clone)**: リモートリポジトリを自分のローカル環境に複製することを指すコマンドです。リモートリポジトリの全ての履歴やファイルを自分のPCにコピーし、ローカルリポジトリを作成します。

- **プル(pull)**: リモートリポジトリから最新の変更を取得して自分のローカルリポジトリを更新する操作です。

- **アド(add)**: Gitで変更をステージングエリアに追加するコマンドです。ステージングエリアに追加された変更は、次のコミットに含まれます。

- **コミット(commit)**: ステージングエリアにある変更をローカルリポジトリに記録するコマンドです。コミットは、変更の履歴を残し、特定のスナップショットを作成します。

- **プッシュ(push)**: ローカルリポジトリの変更をリモートリポジトリに反映するコマンドです。変更をリモートリポジトリに送信し、共有されたプロジェクトに変更を適用します。<br><br>
<img src="https://github.com/2024Web1/web_app_dev/blob/main/git/images/git_image.jpg?raw=true">
