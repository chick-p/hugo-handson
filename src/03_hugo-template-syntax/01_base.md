# シンタックスの基本

HUGO のテンプレート構文についての概要を説明します。

---

## 概要

HUGO のテンプレートで HTML を生成する機能は、GO 言語のテンプレートエンジンを利用しています。
そのため、テンプレートファイルは HTML とテンプレート記法を組み合わせて記述します。

## {{ }}

テンプレートファイル内で、HUGO の変数や関数は `{{ .変数名 }}` や `{{ .関数名 引数1 引数2 }}` のように記述します。
HUGO では、`{{ }}`（mustache 記法）で囲まれた中身をテンプレートエンジンでの処理結果を置き換えて HTML に出力します。

`{{ }}` 内の前のスペースは無くても動きます。ただ、可読性を高めるためにつけたほうが良いとおもっています。

以下は、テンプレートの記述と結果の例です。

### 例1

#### テンプレート

    :::hugo
    <h1>{{ .Site.Title }}</h1>

#### 出力結果

    :::html
    <h1>My New Hugo Site</h1>

`.Site` はサイト設定を表すオブジェクトです（参照: [Site Variables](https://gohugo.io/variables/site/)）。
サイト設定のオブジェクトの変数 `Title` を出力します。

### 例2

#### テンプレート

    :::hugo
    <h1>{{ .Site.Title }}</h1>
    <ul>
      {{ range .Site.RegularPages }}
      <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
      {{ end }}
    </ul>

#### 出力結果

    :::html
    <h1>My New Hugo Site</h1>
    <ul>
      <li><a href="/sample/">Sample</a></li>
    </ul>

`{{ range コレクション名 }} ~ {{ end }}` はループ関数です（参考：[Functions - range](https://gohugo.io/functions/range/)）。
引数として与えられたコレクション名（配列）をひとつずつ取り出し、取り出した要素に対し `{{range}}{{end}}` で囲った中で処理をします。

## ドット（`.`）が指すオブジェクト

HUGO のテンプレートでは、`.` を使って変数にアクセスします。
このドットが指すオブジェクトは、コンテキスト（＝表示する場所）によって内容が異なります。

たとえば、`index.html` はサイトのトップページ（ホームページ）なので、 `.` はトップページのオブジェクトを指しています。

    :::hugo
    <h1>{{ .Site.Title }}</h1>

`single.html` は記事ページのテンプレートなので、`.` は、それぞれの記事ページのオブジェクトを指しています。

    :::html
    <h1>{{ .Title }}</h1> <!-- .Title はその記事ページのタイトルを指す -->
    {{ .Content }} <!-- .Title はその記事ページのコンテンツを指す -->

`index.html` が次に示す内容だった場合、`{{ .Site.Title }}` `{{ range .Site.RegularPages }}` と、`{{ .RelPermalink }}` `{{ .Title }}` の `.` が指す中身は異なります。

    :::hugo
    <h1>{{ .Site.Title }}</h1> <!-- .Site の . はトップページ -->
    <ul>
      {{/* range は繰り返し関数で、RegularPages はサイト内の記事ページ一覧を指します */}}
      {{ range .Site.RegularPages }}  <!-- .Site の . はトップページ -->
        <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
        {{/* .RelPermalink や .Title の . は取り出した要素なので、個々の記事ページを指す */}}
      {{ end }}
    </ul>

`$.` はコンテキストに関わらず、現在のページのオブジェクトを指します。たとえばループ内でもページタイトルを呼び出したいというときに使います。
