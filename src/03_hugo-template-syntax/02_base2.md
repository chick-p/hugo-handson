# テンプレートのシンタックス その2

---

## 変数

ページの種類によって、`.` から呼び出せる変数は異なります。

これらは [Templates](https://gohugo.io/templates/) で確認できます。

- トップページのテンプレートで使える変数：[Homepage Templates](https://gohugo.io/templates/homepage/)
    - [page variables](https://gohugo.io/variables/page/)
    - [site variables](https://gohugo.io/variables/site/)
- 記事のテンプレートで使える変数：[Single Page Templates](https://gohugo.io/templates/single-page-templates/)
    - [page variables](https://gohugo.io/variables/page/)
    - [site variables](https://gohugo.io/variables/site/)
- 一覧のテンプレートで使える変数：[List of Content in Hugo](https://gohugo.io/templates/lists/)
    - [page variables](https://gohugo.io/variables/page/)

## カスタム変数
HUGO にはユーザー独自のカスタム変数を作ることもできます。

カスタム変数は `$変数名 := 値` のように定義します。

    :::html
    {{ $address := "東京都" }} <!-- 定義 -->
    {{ $address }}<!-- 呼び出し -->

値の再代入は `$変数名 = 値` で行います。

    :::html
    {{ $pageType := "通常のページ" }}
    {{ if .IsHome }}
        {{ $pageType = "ホームページ" }}
    {{ end }}
    ここは{{ $pageType }}です。

## 空白のトリム
`{{ }}` 内の前後に `-`（ハイフン）をつけると、ハイフン前後の空白をトリムします。
テンプレートでは可読性のために改行や入れ子にしたいけど、出力したHTMLでは改行や空白なしに出力したいなどの場合に便利です。

### 例1

#### テンプレート

    :::html
    <title>
    {{ if not .IsHome }}{{ .Title }} | {{ end }}{{ .Site.Title }}
    </title>

#### 出力結果

    :::html
    <title>
    記事のタイトル | サイトタイトル
    </title>

### 例2

#### テンプレート

    :::html
    <title>
    {{- if not .IsHome }}{{ .Title }} | {{ end }}{{ .Site.Title }}
    </title>

#### 出力結果

    :::html
    <title>記事のタイトル | サイトタイトル
    </title>

### 例3

#### テンプレート

    :::html
    <title>
    {{- if not .IsHome }}{{ .Title }} | {{ end }}{{ .Site.Title -}}
    </title>

#### 出力結果

    :::html
    <title>記事のタイトル | サイトタイトル</title>

## 関数

HUGO には数値計算や文字列処理を行う関数があります。

基本的なフォーマットは、`{{ 関数名 引数 引数 …}}` です。

#### テンプレート

    :::html
    {{ add 1 2 }}
    {{ lt 1 2 }} <!-- Less Than (<) -->

#### 出力結果

    :::html
    3
    true

利用できる関数は [Functions Quick Reference](https://gohugo.io/functions/) で確認できます。
