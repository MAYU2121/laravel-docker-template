# Laravel Lesson レビュー②

## Todo編集機能

### @method('PUT')を記述した行に何が出力されているか
`<input type="hidden" name="_method" value="PUT">`
HTMLの仕様ではmethod属性はGETとPOSTのみ。疑似的にPUTという形にして解釈させている。

### findメソッドの引数に指定しているIDは何のIDか
Route::で記述した`get('/todo/{id}',`のようなルートパラメーター

### findメソッドで実行しているSQLは何か
SELECT * FROM todos where id = ? limit 1;

### findメソッドで取得できる値は何か
特定のIDとそれに対応するレコード

### saveメソッドは何を基準にINSERTとUPDATEを切り替えているのか
引数にidを指定しfindで取得することでそれに紐づくデータを取得してUPDATEの処理としている

## Todo論理削除

### traitとclassの違いとは
トレイトで作った関数はどこのクラス内でも使用することができる。
また、トレイトはインスタンス化不可、クラスは可能。
### traitを使用するメリットとは
１つのクラスに複数のトレイトを使用可能。
コードの再利用化や共有ができる
## その他

### TodoControllerクラスのコンストラクタはどのタイミングで実行されるか
TodoControllerがインスタンス化されたとき。
各Route::の設定で、TodoController@???としているので指定のURLに遷移したときに実行される。

### RequestクラスからFormRequestクラスに変更した理由
RequestクラスではHTTPメソッドのPOSTやGETにアクセスするために使用。
FormRequestクラスはRequestクラスを継承しており、バリデーション機能を簡単に記述することができる。

### $errorsのhasメソッドの引数・返り値は何か
引数はcontent。(inputのname属性が入る)
返り値はbool型のメソッドなのでtrueかfalse.
contentに対してapp/Http/Requests/TodoRequest.phpで決めたrulesの内容が適応され、空白もしくは255文字以上ならエラーが表示される（true）

### $errorsのfirstメソッドの引数・返り値は何か
まずはhasによって値の有無を確認する必要がある。
引数はhasと同じくinput属性のname。
返り値はhasがtrueだった場合、TodoRequest.phpのmessageメソッドで指定したエラーメッセージの文字列を返す。

### フレームワークとは何か
誰でも一定品質のプロダクトを作成できる枠組み。
laravelではあらかじめファイルのディレクトリ構造やクラスが決まっており、効率よく開発を進めることができる。

### MVCはどういったアーキテクチャか
Model:DBとのやりとり  
View:画面の生成  
controller:Modelとviewの制御、処理  
この３つの頭文字から取っている。またLaravelではroutingもあり、urlと処理（controller）を紐づけている。
WEBクライアントからのリクエストWEBサーバーが受けて返すまでの流れに役割をつけて効率よく開発ができる。

### ORMとは何か、またLaravelが使用しているORMは何か
LaravelではEloquent ORMを使用している。
ORMとはClassとデータベースのテーブルをマッピング（関連付け）することでSQLを直接操作することなく
DBとやり取りを行うことができる機能。

### composer.json, composer.lockとは何か
composer.json:依存するパッケージを定義するためのファイル  
composer.lock:composer.jsonに書かれた依存関係を実際にどのバージョンでインストールしたかを記録するファイル

### composerでインストールしたパッケージ（ライブラリ）はどのディレクトリに格納されるのか
vender