# オブジェクト指向プログラミング①

- [オブジェクト指向プログラミング①](#オブジェクト指向プログラミング)
  - [事前準備](#事前準備)
  - [オブジェクト指向プログラミング](#オブジェクト指向プログラミング-1)
    - [クラス（class）](#クラスclass)
    - [プロパティ（property）](#プロパティproperty)
    - [メソッド（method）](#メソッドmethod)
    - [コンストラクタ（constructor）](#コンストラクタconstructor)
    - [アクセス修飾子](#アクセス修飾子)
    - [継承（inheritance：インヘリタンス）](#継承inheritanceインヘリタンス)
  - [【参考】PHPのオブジェクト指向プログラミングについて、概要を学ぶ方法](#参考phpのオブジェクト指向プログラミングについて概要を学ぶ方法)
  - [本章でやること](#本章でやること)
  - [クラス構成](#クラス構成)
    - [DbDataクラス](#dbdataクラス)
    - [DbPhpクラス](#dbphpクラス)
  - [付録: `require_once`とマジック定数 `__DIR__`](#付録-require_onceとマジック定数-__dir__)

## 事前準備

[こちらのページ](https://classroom.github.com/a/mcVSbvMn)から、ソースコードを`C:¥web_app_dev`へcloneしてください。

## オブジェクト指向プログラミング

PHPは「PHP５」からオブジェクト指向プログラミング言語としての機能が大幅に強化されました。
この資料では、PHPでのオブジェクト指向プログラミングの方法について取り上げますが、その前提として必要なキーワードを以下に示します。

オブジェクト指向プログラミング（Object Oriented Programming: OOP）とは、プログラムを手順ではなくて、モノ（オブジェクト）の作成と操作として見る考え方です。
人によって説明がバラバラで明確な答えはないのでざっくりと理解しておけばよいです。

### クラス（class）

クラスとはオブジェクトの設計図のようなものです。
オブジェクトの中の「プロパティ」、「メソッド」、「コンストラクタ」などが記述されています。

### プロパティ（property）

オブジェクトが持っているデータのことで、クラス内では**変数**として定義します。
「車」というオブジェクトを考えると、「メーカー」、「排気量」、「色」といったプロパティを持ちます。

### メソッド（method）

オブジェクトが持っている処理（振る舞い）のことで、クラス内では**関数**として定義します。
「車」というオブジェクトだと、「走る」、「曲がる」、「止まる」、「燃費を計算する」などの動作をメソッドといいます。

### コンストラクタ（constructor）

オブジェクトの設計図である「クラス」からオブジェクトを作成するときのメソッドのようなものです。（厳密にはコンストラクタとメソッドは別物）
この時作られるオブジェクトのことを「インスタンス」といいます。

### アクセス修飾子

クラス内に定義されたプロパティやメソッドにどこからアクセスできるかを指定するものです。
「public」「protected」「private」のどれかを指定します。
Javaと違って「無指定（デフォルト）」はありません。

- public: クラス内、クラス外のどこからでもアクセス可能
- protected: 同じクラス、及び子クラスからアクセス可能
- private: 同じクラス内からのみアクセス可能

### 継承（inheritance：インヘリタンス）

特定のクラスの機能を引き継いで使うことを継承と言います。
クラスは設計図なので、共通部分は引き継いで、独自のプロパティ、メソッドを追加して新たな設計図を作成します。

## 【参考】PHPのオブジェクト指向プログラミングについて、概要を学ぶ方法

[paizaラーニング「PHP入門編8:クラスを理解しよう」の無料版2本](https://paiza.jp/works/php/primerfemale/beginner-php8-female) ※左記リンクの「01:クラスについて学習しよう (3分31秒)」、「02:クラスを作成しよう (4分40秒)」を参考にしてください。

## 本章でやること

ここでは、[データベースの利用](../db-crud/README.md) で学んだ5本のプログラムをオブジェクト指向を利用したコードに書き換えます。
テーブルは、前章同様`person`を使用します。

## クラス構成

データベースの基本事項に関するクラス `DbData` とそれを継承して実際にデータを操作するクラス `DbPhp` を定義します。
なお、PHPのクラスは **１つのPHPファイルに１つのPHPクラスの定義を記述する** というルールが一般的であり、これに準じて記述していきます。

なお、これから作成する2つのファイル`dbdata.php`、`dbPhp.php`につきましては、`classes`というディレクトリを作成し、その中に作成していってください。

それではまず、クラス `DbData` を定義するPHPファイル `dbdata.php` を以下に示します。

### DbDataクラス

`classes/dbdata.php`

```php
<?php
// DbDataクラスの宣言 ・・・①
class DbData
{
  // PDOオブジェクト用のプロパティ(メンバ変数)の宣言 ・・・②
  protected $pdo;

  // コンストラクタ
  // 「__construct」の「̲̲__」は「_(アンダースコア)」を2つ記述する ・・・③
  public function __construct()
  {
    // PDOオブジェクトを生成する
    $user = 'sampleuser';
    $password = 'samplepass';
    $host = 'db';
    $dbName = 'SAMPLE';
    $dsn = 'mysql:host=' . $host . ';dbname=' . $dbName . ';charset=utf8';
    try {
      $this->pdo = new PDO($dsn, $user, $password); // ④
    } catch (Exception $e) {
      // 接続できなかった場合のエラーメッセージ
      exit('データベースに接続できませんでした：' . $e->getMessage());
    }
  }

  // SELECT文実行用のquery( )メソッド ・・・このメソッドはユーザー定義関数
  protected function query($sql, $array_params) // ⑤
  {
    $stmt = $this->pdo->prepare($sql);
    $stmt->execute($array_params);
    // PDOステートメントオブジェクトを返すので
    // 呼び出し側でfetch( )、またはfetchAll( )で結果セットを取得
    return $stmt;
  }

  // INSERT、UPDATE、DELETE文実行用のメソッド ・・・このメソッドもユーザー定義関数
  protected function exec($sql, $array_params) // ⑥
  {
    $stmt = $this->pdo->prepare($sql);
    // 成功:true、失敗:false
    $stmt->execute($array_params);
  }
}
```

①: `class DbData {`

PHPでクラス宣言を行う場合の書き方です。
ここで宣言したクラス内でプロパティ（メンバ変数）やコンストラクタ、メソッドを定義します。

②: `protected $pdo;`

PHPのアクセス修飾子は、以下の３つです。（Javaはこれ以外にアクセス修飾子を宣言しない「デフォルト」があります）

- `public`: クラス内、クラス外のどこからでもアクセス可能
- `protected`: 同じクラス、及び子クラスからアクセス可能
- `private`: 同じクラス内からのみアクセス可能

今回はクラス`DbData`を継承した子クラス`DbPhp`でこの `$pdo` プロパティ（メンバ変数）を利用するので、`protected` を使用して宣言しておきます。

③: `public function __construct( ) {  }`

「コンストラクタ」とは、クラスからオブジェクトが `new` によって作成される時に自動的に呼び出されるメソッド(のようなもの)です。
コンストラクタは他のクラスから利用できるようにアクセス修飾子は `public` を使用して宣言します。
ここでは、データベースphpに接続するために `$dsn、$user、$password` の初期値を設定し、PDOオブジェクトを生成しています。

④: `$this->pdo = new PDO( $dsn, $user, $password );`

クラスのプロパティを定義し、そのクラスのメソッド内で参照したい場合、単に `$プロパティ名` で使用できません。
クラス内のプロパティにアクセスするためには、**疑似変数 `$this`** を使用し、`$this->プロパティ名` で使用します。(「->」を「シングルアロー」、「アロー演算子」という。)

例えば、プロパティとして `$name` を定義し、そのプロパティに値をセットする `setName( )` というメソッドを定義した場合、以下のように記述します。

```php
$name = ""; // プロパティとして宣言したメンバ変数

public function setName( $name ) { // この$nameはメソッドsetName( )の仮引数
  $this->name  =  $name;  // 「$this->name」がプロパティで宣言した「$name」 
} 　                      // 右辺にある「$name」はこのメソッドsetName( )の仮引数
```

⑤: `protected function query ( $sql, $array_params ) {  ｝`

この`DbData`クラスを継承したサブクラスで「SELECT文」を実行するときに呼び出すメソッドとして定義します。
継承したサブクラスでも利用できるようにアクセス修飾子 `protected` で宣言します。
２つの引数の内容は以下のとおりです。

- `$sql`: プレースホルダー（「?」）を利用して定義したSQL文
- `$array_params`: プレースホルダー（「?」）に代入する値を定義した配列データです。
実行結果のPDOステートメントオブジェクトを`return`文で返し、呼び出した側で `fetch( )` または `fetchAll( )` を使用して個々のデータの処理を行います。

⑥: `protected function exec ( $sql, $array_params ) {　}`

この`DbData`クラスを継承したサブクラスで「UPDATE文」、「INSERT文」、「DELETE文」を実行するときに呼び出すメソッドとして定義します。

この `exec( )` メソッドも継承したサブクラスでも利用できるようにアクセス修飾子 `protected` で宣言します。
２つの引数の内容は、⑤の`query( )` メソッドと同じです。

実行結果は、成功した場合の「true」または失敗した場合の「false」であるので、呼び出した側で必要に応じて処理を記述します。

**なお、ファイル内の言語がphpのみで構成されている(HTMLを含まない)場合、最後の `?>` の記述は不要です。**

### DbPhpクラス

次に、**クラス「DbData」を継承する、クラス「DbPhp」を定義するPHPファイル「dbphp.php」** を作成します。
このクラス「DbPhp」には、次の５つのメソッドを定義します。

① `selectAll( )` メソッド</br>
テーブルpersonのすべてのデータを抽出するメソッド

② `selectPerson( $uid )` メソッド</br>
引数で指定されたユーザーID（$uid）のデータを抽出するメソッド

③ `insertPerson( $name,  $cid,  $age )` メソッド</br>
引数で渡された氏名、カンパニーID、年齢の値で新規ユーザーを登録するメソッド

④ `updatePerson( $uid,  $name )` メソッド </br>
引数で指定されたユーザーID($uid）の氏名を引数で渡された$nameの値に更新するメソッド

⑤ `deletePerson( $name )` メソッド</br>
引数で指定された氏名（$name）のデータを削除するメソッド

`classes/dbphp.php`

```php
<?php
// DbDataクラスを利用するため
// 「DIR」の前後には2本のアンダースコア!!
require_once __DIR__ . '/dbdata.php';

// DBDataクラスを継承したDbPhpクラスの宣言
class DbPhp extends DbData
{
    // ①: テーブルpersonからすべてのデータを抽出する
    public function selectAll()
    {
        $sql = "select * from person";
        // 継承したDBDataクラスのquery( )メソッドを呼び出している
        // SQL文にプレースホルダはないので空の配列を渡している
        $stmt = $this->query($sql, []);
        $persons = $stmt->fetchAll();
        // 抽出した複数のデータを連想配列の形式で戻り値とする
        return $persons;
    }

    // ②:テーブルpersonから指定されたuidのデータを抽出する
    public function selectPerson($uid)
    {
        $sql = "select * from person where uid = ?";
        // 継承したDBDataクラスのquery( )メソッドを呼び出している
        // SQL文のプレースホルダは1つだけだが配列の形式で渡す
        $stmt = $this->query($sql, [$uid]);
        $person = $stmt->fetch();
        // 抽出した1件のデータを連想配列の形式で戻り値とする
        return $person;
    }

    // ③:テーブルpersonに新規ユーザーを登録する
    public function insertPerson($name, $cid, $age)
    {
        $sql = "insert into person ( name, company_id, age ) values ( ?, ?, ? )";
        // 継承したDBDataクラスのexec( )メソッドを呼び出している
        // SQL文のプレースホルダの数だけ配列で渡す
        $this->exec($sql, [$name, $cid, $age]);
    }

    // ④:テーブルpersonのuidを指定し、氏名の値を更新する
    public function updatePerson($uid, $name)
    {
        $sql = "update person set name = ? where uid = ?";
        // 継承したDBDataクラスのexec( )メソッドを呼び出している
        // SQL文のプレースホルダの数だけ配列で渡す
        $this->exec($sql, [$name, $uid]);
    }

    // ⑤:テーブルpersonの氏名を指定し、データを削除する
    public function deletePerson($name)
    {
        $sql = "delete from person where name = ?";
        // 継承したDBDataクラスのexec( )メソッドを呼び出している
        // SQL文のプレースホルダは1つだけだが配列の形式で渡す
        $this->exec($sql, [$name]);
    }
}
```

`class DbPhp extends DbData {`

これにより`DbData`クラスで宣言した `$pdo` プロパティ、`query( )` メソッド、`exec( )` メソッドをこのクラスが利用できます。

なお、dbdata.php同様、ファイル内の言語がphpのみの場合、最後の `?>` の記述は不要です。

**dbdata.php, dbphp.phpのプログラムを作成しても、動作確認はまだできません。またGitHubにもpushはしないでください。動作確認、およびGitHubへのpushは、次章の「オブジェクト指向プログラミング②」にあるプログラムを作成する必要があります。**

## 付録: `require_once`とマジック定数 `__DIR__`

開発の規模が大きくなり、全体のコード量が膨大になると、それぞれのファイルに同じコードを記述するのは 非効率かつ保守性に問題がああります。
そこで、`require_once`を使い、共通的な処理を記述しているファイルを読み込むことで、効率化・保守性の向上を図る。

前述の「dbphp.php」では、次のように記述されている。

```php
require_once  __DIR__ . '/dbdata.php'; // 【注】__DIR__の「__」部分はアンダースコア2本!!
```

読み込むファイル名は絶対パスでも相対パスでもよいのだが、`__DIR__` を用いて絶対パスで指定します。
`__DIR__` はPHPのマジック定数（PHPの定義済み定数）の１つで、そのファイルの存在するディレクトリを返します。
なお、`__DIR__` の「`__`」部分はアンダースコア（ `_`）を２つ連続して記述するので注意してください。

[【参考ページ】PHPのマジック定数](http://php.net/manual/ja/language.constants.predefined.php)

ファイルを読み込む方法として他にも `require`、`include_once`、`include`などもありますが、下記の参考サイトで その違いなどを確認しておいてください。

[PHP requireとrequire_onceの違い](https://zeropuro.com/blog/?p=322)</br>
[PHP requireとincludeの違い](https://zeropuro.com/blog/?p=328)

|命令|特徴|
| - | - |
|require|指定されたファイルを何度でも読み込む。読み込めない場合、エラーとなる|
|require\_once|指定されたファイルを一度だけ読み込む。読み込めない場合、エラーとなる。|
|include|指定されたファイルを何度でも読み込む。読み込めない場合、警告となる|
|include\_once|指定されたファイルを一度だけ読み込む。読み込めない場合、警告となる。|

