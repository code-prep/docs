# CODEPREP Bookの作り方
## 概要
CODEPREPのBookはそのほとんどがMarkdownで作成できるように設計されています。  
この文書ではBookを構成する各種ファイルの詳細について説明します。

## 最小限のBook
説明に先立ってまず最小限のBookの例を示します。

以下は

- book.yml
- chapter1.md

の２ファイルからなる、1チャプター、1セクションのBookの例です。

注. 例の中ではマークダウンのコードブロックが`\\`\`\``のように示されています。

book.yml
```
title: はじめてのCODEPREP
detail: CODEPREPのBookのサンプルです。
chapters:
  - chapter1.md
```

chapter1.md
```
# 最初のチャプター
チャプター定義のサンプルです。

## 最初のセクション
セクション定義のサンプルです。
ブランクを埋めて「Hello World」と出力されるHTMLを完成させてください。

### main(index.html)

\```
<p>Hello ${World}</p>
\```

```

この２ファイルを[Preview App](README.md)を通して見ると「<p>Hello [     ]</p>」というページが表示されます。

以下それぞれのファイルについて説明していきます。

## book.yml
book.ymlはBook全体の定義が記述されたファイルです。

以下のキーを含みます。

- title: String, 必須
- detail: String, 必須
- chapters: List[String], 一つ以上必須
- image: String, 任意
- files: List[String], 任意

### title
titleにはBookのタイトルを指定します。

### detail
detailにはBookの概要説明を指定します。  
複数行指定したい場合はYamlのパイプ(|)を使用してください。

```
detail: |
  Bookの説明です。
  パイプ(|)を使用して複数行で記述しています。
```

### chapters
chaptersには各チャプターの定義ファイルを指定します。  
指定したファイルは必ずそのパスに存在しなければなりません。

リストの順番がそのままチャプターの順序になります。

チャプター定義ファイルの名前は任意であり、サブディレクトリに置くこともできます。

### image
imageにはBookのカバー画像のファイルパスを指定します。  
指定したファイルは必ずそのパスに存在しなければなりません。

imageを省略してもコンパイルエラーとはなりませんが、Bookの公開までには必ず画像ファイルを用意してください。

### files
book.ymlのfilesにはすべてのチャプター、セクションで横断的に使用するファイルを指定します。  
ここで指定されたファイルはBookReaderのエディタ部でタブとして表示されます。
指定したファイルは必ずそのパスに存在しなければなりません。

以下のような場合にはbook.ymlではなく、個別のセクションのfilesサブセクションで指定するようにしてください。

- ファイル(例: main.css)が1章では参照されないが、2章以降で参照される場合
- ファイルの内容が途中で変わる場合

以下のようにマークダウンのリンク形式で記述することにより実際のファイルパスと表示上のファイル名を変更することができます。

```
- [main.css](files/something.css)
```

## チャプター定義ファイル
チャプター定義ファイルはMarkdown形式で記述されたチャプターの定義ファイルです。  
CODEPREPではひとつのチャプターがひとつのファイルに対応し、その中に複数のセクションを含めることができます。

### チャプター定義ファイルの見出し
チャプター定義ファイルではマークダウンの見出し(#, ##, ###)を各種定義の区切りとして使用しています。  
それぞれの見出しは以下の意味を持ちます。

- 見出し1(#)はチャプターのタイトルを示します。
  - チャプタータイトルは自由につけることができます。
  - チャプター定義ファイルは必ず見出し1(#)で始まらなければなりません。
  - 見出し1(#)はチャプター定義ファイル内に複数定義してはいけません。
- 見出し2(##)はセクションのタイトルを示します。
  - セクションタイトルは自由につけることができます。
  - 見出し2(##)はチャプター定義ファイル内で複数定義することができます。
- 見出し3(###)はサブセクションを示します。
  - 見出し3(###)の名前は「main」「hint」「preview」等固定です。
  - 見出し2(##)に続く複数の見出し3(###)がセクションの内容となります。

未定義の名前を持つ見出し3、見出し4-見出し6はチャプター定義ファイルでは使用できません。  
存在する場合はコンパイルエラーとなります。

### チャプター定義ファイルのコメント
コードブロック以外で「//」で始まる行はコメントとして扱われるため、Bookの内容には含まれません。

この行はMarkdownの書式上でのコメント行ではないので、GitHub等のMarkdownViewer上ではそのまま表示されます。

例
```
# チャプター１

// ToDo あとで修正する

チャプター1の説明です。

```

逆にMarkdown書式のコメント「`<!-- -->`」はMarkdownViewer上では表示されませんが、Bookのコンパイルの際にはそのままのテキストとして扱われます。

### チャプタータイトルと説明
チャプター定義の先頭には必ず見出し1がきます。  
そして、その次に来る見出しは必ず見出し2(セクションタイトル)になります。

チャプタータイトルの下にはMarkdownで自由にチャプターの説明を記述することができます。  
(ただし、見出しは使用できません。)

注. 現在のBookReaderではチャプターの説明が表示されることはありません。(必要ならば改修します。)

### セクションの構成
セクションタイトルの下にはMarkdownで自由にセクションの説明を記述することができます。  
(ただし、見出しは使用できません。)

この説明はBookReaderの画面左に表示される問題文になります。

セクションは以下のサブセクションを持つことができます。

- main
- hint
- tips
- files
- preview
- remote

mainサブセクションは必須です。  
それ以外のサブセクションは必要に応じて定義します。

### mainの定義
mainサブセクションの定義は見出し3(###)で「main(ファイル名)」という形式で行います。

mainサブセクションではコードブロックが必須であり、コードブロック内で一つ以上のAnswer定義が必須です。

```
### main(index.html)

\```
<${p}>Hello World<${\/p}>
\```
```

Answerは通常、`${...}`という形式で定義し、最もシンプルなケースでは{}で囲まれた文字列がそのまま正答となります。(大文字小文字を区別します。)

ただし、大文字小文字の区別をしないなど追加の要件に対応するために{}内の先頭文字によって以下のように解釈が変わります。

#### 先頭が「/」の場合
{}内は正規表現であると解釈されます。

例
- ${/red/i}  - 大文字小文字の区別なしで「red」にマッチ
- ${/abc|def/} - 大文字小文字の区別ありで「abc」または「def」にマッチ

JavaScriptのRegExがサポートする範囲で自由に正規表現を定義することが可能です。
文字列が正規表現として不正な場合はコンパイルエラーとなります。

注.   
正規表現内でOR(|)や繰り返し(\*)等を使用した場合、解答の最大長が計算できなくなります。  
通常、BookReaderで表示されるブランクではwidthと入力のmaxlengthが解答の最大長に基づいて設定されますが、この場合は

- width: 8文字分
- maxlength: 設定なし

となります。

maxLengthを適切に設定したい場合は次節のJSON定義を使用してください。

#### 先頭が「{」の場合
{}内はJSONであると解釈されます。

例
- ${{"text": "hoge", "caseSensitive": true, "defaultValue": "fuga"}}
- ${{"regexp": "hoge|fuga", "flags": "i", "maxLength": 4}}

JSON内で定義できるキーは以下です。

- text: 正答となる文字列。textまたはregexpのいずれかは必須
- regexp: 正答となる正規表現。textまたはregexpのいずれかは必須
- caseSensitive: 大文字小文字を区別するかのフラグ。textの場合のみ有効
- flags: 正規表現のフラグ。例えば大文字小文字を区別しない場合は"i"を指定する。regexpの場合のみ有効
- maxLength: 解答の最大長。regexpの場合のみ有効
- defaultValue: 初期値。指定した場合最初から誤答がブランクに設定される。

上記以外のキーは無視されます。

#### 先頭が「\」の場合
先頭の「\」を飛ばして次の文字以降をシンプルな文字列として解釈します。

例
- ${\/p}
- ${\{}

この設定は文字列が正規表現、あるいはJSONと解釈されるのを防ぎたい場合に有効です。

注. 
この機能はエスケープではなくあくまで先頭の「\」をスキップするだけの機能です。 
シンプルな文字列と解釈される場合のエスケープ機能はありません。

#### `$`の別の文字への変更
`${xxx}`という書式はES6やScalaでは構文として使用されています。  
このようなケースに対応するために、prefixを定義して`$`を別の一文字に変更することができます。

```
### main(index.html)

- prefix: %

\```
<%{p}>Hello World<%{\/p}>
\```
```

prefixに二文字以上の文字列を指定した場合でも有効となるのは最初の一文字のみです。

#### aliasの定義
mainサブセクションではオプションとしてalias(別名)を指定できます。

```
- alias: index1
```

ここで設定したaliasは後述する関数から参照できます。

aliasで使用できる文字は `[0-9a-zA-Z_-.]` です。

### hint/tipsの定義
「### hint」または「### tips」という見出し3の下にMarkdownで任意の文章を定義することができます。

hintあるいはtipsはデフォルトでは表示されず、ユーザーアクションによって表示される文字列です。

hintとtipsには機能的な差異は存在しないので用途によって使い分けてください。(両方定義することも可能ですが、BookReaderの見栄え的にあまりお勧めはしません。)

### filesの定義
filesには参照する外部ファイルをリスト形式で複数設定できます。
ここで指定したファイルはエディタ部でタブとして表示されます。
指定したファイルが存在しない場合はコンパイルエラーとなります。

例
```
### files

- main.css
- [main.js](chapter1/section1_main.js)
```

Markdownのリンク書式を使用して実際のファイル名と表示上のファイル名を変更することも可能です。

### previewの定義
プレビュー(BookReaderの画面右側下半分に表示される)は通常、後述のロジックに従って自動生成されますが独自のプレビューを
ベット定義することができます。

previewではコードブロックでインラインにpreviewを定義するか、リスト形式で

```
- file:chapter1/section1_preview.html
```

のように外部ファイルを参照することができます。

preview機能は必ずしもmainの内容をそのまま再現する必要はなく、自由にカスタマイズすることができます。

例
```
### main(chapter1.md)

\```
### ${preview}
\```


### preview

\```
<html>
<head>
  <title>Preview</title>
  <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script>
$(document).ready(function() {
  var answer = "${a1}";
  if (answer === "preview") {
    $("#preview").show();
  }
});
</script>
</head>
<body>
<p id="preview" style="display:none;">この文章はブランクを正しく埋めると表示されます。</p>
</body>
</html>
\```
```

上の例ではユーザーの解答が正しい場合に説明文を表示しています。

preview定義内では以下のキーワードが置換文字列として使用できます。

- ${a1}, ${a2}... ユーザーの入力したブランクの解答。1ベースの序数で${a1}が最初のブランクに対応
- ${content} mainの内容全体(ユーザーのブランク入力もそのまま反映されます。)

また、コードブロック(file)とは別にオプションとして以下が設定できます。

- contentType: プレビューのContent-Type。省略時は text/html
- prefix: キーワード参照に使用するprefix。省略時は$。

例
```
### preview

- contentType: text/plain
- prefix: %

\```
あなたの解答は%{a1}です。
\```
```

### remoteの定義
remoteはRubyなどのサーバーサイドの言語を実行する機能です。  
commandに実行するコマンドを定義します。

```
### main(ruby.rb)
\```
${puts} "Hello World"
\```

### remote
- command: ruby main.rb
```

上の例ではmainでユーザーが作成したmain.rbを実行しています。(Hello Worldが表示されます。)

またオプションとして以下が設定できます。

- mode: 実行モード。`console`または`html`。省略時はconsole
- build: command実行前に実行する準備のコマンド。複数指定可
- file: 実行時に追加でサーバーにコピーするファイル。複数指定可

#### remote実行時にコピーされるファイル
remote実行時には以下のファイルが実行サーバーにコピーされます。

- main(mainに指定されたファイル名でコピーされます。)
- book.ymlのfilesに指定されたファイル
- セクションのfilesサブセクションに指定されたファイル
- remoteサブセクションでfileオプションとして指定されたファイル

コピー先のファイル名は別名が定義されている場合はそちらが使用されます。

```
### remote

- build: npm install
- command: node main.js
- file: [package.json](chapter1/package.json)

```

上の例では「npm install」を実行してからnodeが実行されます。

Book定義時に実際にファイルが存在するパスは「chapter1/package.json」ですが、コピー先はコマンドを実行するrootディレクトリ上の「package.json」となります。

(filesサブセクションで指定したファイルはエディタ部でタブとして表示されますが、remoteで指定されたファイルは非表示でユーザーには提示されないという違いがあります。)

画像ファイルなどサイズの大きなファイルを多数転送するとパフォーマンス低下につながるので注意してください。

#### html mode
PHPのBookなどではコマンドの実行結果がそのままHTMLとなっている場合があります。
その場合は、

```
- mode: html
```

を設定しておくことでコマンド実行後にHTMLでのプレビューを表示させることができます。

### remoteまたはpreviewが未定義の場合のプレビュー
画面右下に表示される内容は以下の優先順位で決定します。

- remoteが定義されている場合はremoteコンソールが表示されます。
- previewが定義されている場合はその内容がpreviewとして表示されます。
- mainのファイル名がindex.htmlの場合はその内容が表示されます。
- filesサブセクションに「index.html」というファイルが指定されている場合はその内容が表示されます。
- いずれにも該当しない場合はmainの内容がtext/plainで表示されます。

なお、mainとfilesで定義されたファイル間でのscript/cssの参照は自動的に解決されます。  
また、HTML内で画像ファイルを相対パスで参照する場合はそのファイルはfilesサブセクション(またはbook.ymlのfiles)に設定されている必要があります。

この際の参照関係の解決はファイル名のみで行われます。

例えば、filesサブセクションに

```
- image1.png
```

が設定されている場合、 HTML内で定義されているimgタグが `<img src="image1.png"/>` でも `<img src="hoge/image1.png"/>` でも同じ画像が表示されます。

## 高度な機能
ここではBookを作成する上で、さらに便利な機能を紹介します。

### main内での先行セクション、外部ファイルの参照
例として、

- セクション1でh1を定義し、
- セクション2でその下にh2を定義し、
- セクション3でその下にpタグを定義する

というケースを考えてみます。

素直に書くとそれぞれのmainの定義は以下のようになります。

Section1
```
<${h1}>This is H1<${\/h1}>
```

Section2
```
<h1>This is H1</h1>
<${h2}>This is H2<${\/h2}>
```

Section3
```
<h1>This is H1</h1>
<h2>This is H2</h2>
<${p}>This is p<${/p}>
```

前の内容をコピーしながら少しずつ内容を書き足していることがわかります。

これでも問題なく動作しますが、メンテナンス性はかなり悪くなります。(最初の方のセクションを修正してしまうとそれ以降のすべてを修正する羽目になります。)

この場合 `${codeprep:section(prev)}` という関数を使用することで直前のセクションの完成した内容を参照することができます。  
この機能によって先の内容は以下のように書き換えることができます。

Section1
```
<${h1}>This is H1<${\/h1}>
```

Section2
```
${codeprep:section(prev)}
<${h2}>This is H2<${\/h2}>
```

Section3
```
${codeprep:section(prev)}
<${p}>This is p<${/p}>
```

注. 
Answerを正規表現で定義している場合、正解を決定できないことがあります。  
例えば `${abc|def}` という正規表現では"abc"と"def"のどちらを正解とするべきか判断できません。  
この場合はコンパイルエラーとなるのでJSON形式でのAnswer定義を使用してください。

このcodeprep:section関数では引数として"prev"以外に以下を取ることができます。

- 数字: 同一チャプター内のセクション番号。
  - 例: `${codeprep:section(2)}` はセクション2の内容を参照します。
- 任意の文字列: 先行するmainで定義されたaliasを検索し参照します。
  - 例: `${codeprep:section(index1)}` は `- alias: index1` を持つmain定義を参照します。

いずれの場合もそのセクションが見つからない場合はコンパイルエラーとなります。


### その他のsectionを参照する関数
先のsection関数は指定のmainの内容をまるごと参照していましたが、追加する内容が前の内容の後ではなく間に挟まることも多々あります。  
こうしたケースに対応するため以下の関数があります。

(下記のSYMBOLはsection関数と同じ引数(prev, 数値, alias)のことです。)

- section-before-blank(SYMBOL)
  - main定義中の最初のブランク出現行を検索し、その前の行までを返します。(ブランク行を含まない)
- section-after-blank(SYMBOL)
  - main定義中の最後のブランク出現行を検索し、その後ろの行を返します。(ブランク行を含まない)
- section-to-blank(SYMBOL)
  - main定義中の最後のブランク出現行を検索し、その行までを返します。(ブランク行を含む)
- section-from-blank(SYMBOL)
  - main定義中の最初のブランク出現行を検索し、その行以降を返します。(ブランク行を含む)

また、任意の文字列を検索してその行を基準に内容を分割する関数もあります。

- section-before(SYMBOL, "検索文字列")
  - main定義から指定の文字列のある行を検索し、その前の行までを返します。(検索行を含まない)
- section-after(SYMBOL, "検索文字列")
  - main定義から指定の文字列のある行を検索し、その後ろの行を返します。(検索行を含まない)
- section-to(SYMBOL, "検索文字列")
  - main定義から指定の文字列のある行を検索し、その行までを返します。(検索行を含む)
- section-from(SYMBOL, "検索文字列")
  - main定義から指定の文字列のある行を検索し、その行以降を返します。(検索行を含む)

こちらの関数では指定の文字列を含む行が2行以上ある場合はコンパイルエラーとなります。

例
```
...
<div>
  <${ul}><${\/ul}>
</div>
...
```

```
${codeprep:section-before-blank(prev)}
  <ul>
    <${li}><${\/li}>
    <${li}><${\/li}>
  </ul>
${codeprep:section-after-blank(prev)}
```

### fileを参照する関数
セクションではなく外部ファイルを参照する関数もあります。  
こちらの関数では第一引数は外部ファイルへのパスとなります。

- ${codeprep:file(filepath)}
- ${codeprep:file-before-blank(filepath)}
- ${codeprep:file-after-blank(filepath)}
- ${codeprep:file-to-blank(filepath)}
- ${codeprep:file-from-blank(filepath)}
- ${codeprep:file-before(filepath, "検索文字列")}
- ${codeprep:file-after(filepath, "検索文字列")}
- ${codeprep:file-to(filepath, "検索文字列")}
- ${codeprep:file-from(filepath, "検索文字列")}

### filesサブセクションでの先行セクションの参照
filesサブセクションでも外部ファイルではなく先行するセクションを参照することができます。

```
### files
- section: prev
```

参照名にはsection関数と同様に

- prev
- 数値(同一チャプターのセクション番号)
- 先行するセクションで定義したalias

が指定できます。

この機能をうまく利用すると、例えばHTMLとCSSを交互に編集しながら組み立てるようなBookを一切外部ファイルの参照なしで組み立てることも可能です。


### 作ってみたけどいきなりDeprecatedな関数
上記以外にもチャプター番号とセクション番号で参照する関数もあります。

- ${codeprep:chapter(chapterIndex, sectionIndex)}
- ${codeprep:chapter-before-blank(chapterIndex, sectionIndex)}
- ${codeprep:chapter-after-blank(chapterIndex, sectionIndex)}
- ${codeprep:chapter-to-blank(chapterIndex, sectionIndex)}
- ${codeprep:chapter-from-blank(chapterIndex, sectionIndex)}
- ${codeprep:chapter-before(chapterIndex, sectionIndex, "検索文字列")}
- ${codeprep:chapter-after(chapterIndex, sectionIndex, "検索文字列")}
- ${codeprep:chapter-to(chapterIndex, sectionIndex, "検索文字列")}
- ${codeprep:chapter-from(chapterIndex, sectionIndex, "検索文字列")}

ただし、aliasを使用すればsection関数を使えば別チャプターも参照できることと、chapter/section番号はBook編集中に容易に変わりえることからあまりオススメはしません。

同様にfilesサブセクションでも

```
- chapter: 1-2
```

というチャプター番号とセクション番号での参照が可能ですが、同じ理由でsectionでのalias参照を推奨します。
