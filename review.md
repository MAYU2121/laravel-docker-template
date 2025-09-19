# Laravel Lesson レビュー①

## Todo一覧機能

### Todoモデルのallメソッドで実行しているSQLは何か
SELECT * FROM todos;

### Todoモデルのallメソッドの返り値は何か
Illuminate\Database\Eloquent\Collectionクラスのインスタンス.
Laravelで用意されている、配列操作に特化したクラス。

### 配列の代わりにCollectionクラスを使用するメリットは
配列の操作を直感的にメソッドチェーンで記載できる

### view関数の第1・第2引数の指定と何をしているか
第一引数は表示したいbladeファイル。viewsからの対象の.blade.phpまでのパス。  
resources/views/todo/index.blade.phpという構造の場合、view('todo.index')となる。  
第二引数は渡したいデータ。[blade内で使う変数名 => 代入したい値]を入れます。
### index.blade.phpの$todos・$todoに代入されているものは何か
todoテーブルのデータ全件
## Todo作成機能

### Requestクラスのallメソッドは何をしているか
フォームから送信された値を、content,name,idとひとつずつ記載して取得するのではなく一括で取得する。

### fillメソッドは何をしているか
allで取得した配列の形を$テーブル名->{連想配列のkey} = {連想配列のvalue}の形にしている。
'$fillable'の設定がある場合、そこで決めた値のみ代入される。

### $fillableは何のために設定しているか
allで全てのカラムに対するデータが取得されてくるが、どのカラムを一括代入の対象にするのかを決める。
もし制限がなくidまで代入されている状態で攻撃者がデベロッパーツールを開くと、そのidを改ざんして他者になりすまし投稿することができてしまう。許可されたものしか受け付けないため、idを入力して上書きしようとしてもはじかれる。

### saveメソッドで実行しているSQLは何か
INSERT INTO todos(content)
fillableでcontentのみ許可している

### redirect()->route()は何をしているか
redirectもrouteもヘルパー関数。
routeで指定したURLに移動させる。
web.phpにてname('todo.index')というようにルートに名前をつけた。これを呼び出す（リンクを生成して使用したい）ためにroute('todo.index');をここで設定している。  


## その他

### テーブル構成をマイグレーションファイルで管理するメリット
SQLの知識がなくても、直感的に実装ができる。
またコードでの管理で他の開発者とデータベースの状態を共有しやすい。

### マイグレーションファイルのup()、down()は何のコマンドを実行した時に呼び出されるのか
up: php artisan migrate
down: php artisan migrate:rollback

### Seederクラスの役割は何か
初期データを入れる

### route関数の引数・返り値・使用するメリット
引数  route(string $name, array $parameters = [], bool $absolute = true)  
返り値  URL。->name()に対応するURL文字列。  
使用するメリット  URLを直接書かなくて済むため可読性が高い。URLが変更された場合、変更する必要がない。

### @extends・@section・@yieldの関係性とbladeを分割するメリット
多くのblade.phpがあって共有の部分があるとき、それをベースとなる親のbladeにまとめて異なる部分は子としてそれぞれのbladeを作成する。  
親bladeには子bladeの内容を挿入したいところに@yieldを置き、子bladeの内容は@section～@endsection内に記載する。@sectionの前には@extends()を置き、()内にはview以降のパスで親ファイルを指定。  
メリットとしては重複がなくなることでの可読性の向上、分担制作のしやすさ。

### @csrfは何のための記述か
form内に@csrfと記述することでCSRF対策が完了する。
生成も検証も自動的に行われる

### {{ }}とは何の省略系か
phpのechoの省略系。
{{ $todo->content }}であれば、  
<?php echo e($todo->content); ?>  
となる。  
また、このようにeがついているようにエスケープも自動的に実装される。
