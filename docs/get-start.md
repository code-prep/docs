## CODEPREP bookをつくってみよう
ここでは簡単なサンプルBookをつくる手順を示します。
### 1. Preview Appのセットアップ
CODEPREP Bookを編集するために、ローカル環境で簡単にBookを動かす(https://github.com/givery-technology/book-codeprep)を提供しています。  

まずは以下のコマンドを実行して、Preveiw Appの環境を構築します。

``` bash
$ git clone git@github.com:givery-technology/codeprep-backend.git
$ git clone git@github.com:givery-technology/book-codeprep.git
$
$ cd book-codeprep
$
$ export CODEPREP_BACKEND_PATH=../codeprep-backend
$ ./setup.sh
```

### 2. サンプルBookのインストール
次に、サンプルアプリをインストールします。  

``` bash
$ cd ../
$ git clone git@github.com:code-prep/sample-book.git
```

### 3. サンプルBookをプレビュー
サンプルBookをPreview Appを使って開いてみましょう。  
Preview Appでサンプルアプリを実行するには、`BOOK_SRCDIR`にサンプルBookを指定する必要があります。

```
$　cd book-codeprep
$ sbt run -DBOOK_SRCDIR=../sample-book
```

`http://localhost:9000/`にアクセスすると、指定されたサンプルBookをプレビューすることが出来ます。  
(また、portを変更するにはコマンドラインにて `-Dhttp.port=8000`のような形で変更することも可能です。)

### 4.サンプルBookでCODEPREP Bookで出来ることを確認してみよう
サンプルBookには、CODEPREP Bookで出来ることが記載されています。  
[CODEPREP bookの作り方](how-to-make-book.md)を参照しながら、あなただけのオリジナルBookを作成してみてください。

## ToDo
Separate BookCompiler from codeprep-backend and get it from some repo.

Then we can provide this app to external authors.
