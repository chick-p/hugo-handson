# 条件分岐

---

## 条件分岐

### if
条件分岐を行うには、`{{ if 条件式 }} ... {{ end }}` のように記述します。

    :::hugo
    {{/* 記事タイトル（frontmatter の title に値）があったら出力する */}}
    {{ if isset .Params "title" }}
      <h1>{{ .Params.Title }}</h1>
    {{ end }}

### if else
`{{ if 条件式 }} ... {{ else }} ... {{ end }}` とすると if-else 式を書けます。

    :::hugo
    {{/* 記事に説明文（frontmatter の description に値）があったら出力し、そうでない場合は記事のサマリーを出力する */}}
    {{ if isset .Params "description" }}
      {{ .Params.Description }}
    {{ else }}
      {{ .Summary }}
    {{ end }}

### and/or
and 式や or 式で複雑な条件式も書けます。条件式は`()`でくくります。

    :::hugo
    {{/* 記事に title か caption が存在し、かつ attr がある場合は出力する */}}
    {{ if (and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")) }}
      title
    {{ end }}

### [with](https://gohugo.io/functions/with/)
「もし ... があれば ... を行う」という条件式を書きたい場合、`with` を使うこともできます。

    :::hugo
    {{ with .Params.title }}
      <h1>{{ . }}</h1> {{/* . の中身は Params.title になっている */}}
    {{ end }}

with else もできます。

    :::hugo
    {{ with .Param "description" }}
      {{ . }} {{/* . の中身は Params.description になっている */}}
    {{ else }}
      {{ .Summary }}  {{/* . の中身は記事（初期値） */}}
    {{ end }}

## 比較演算子
条件分岐では `eq`(equal) や `ne`(not equal) などの関数や、比較演算子（`==`, `!=`）を利用できます。

- 関数を使う場合  
  `{{ 関数 値 比較対象 }}`

        :::hugo
        {{ if eq .Language "ja"}} ... {{ end }}

- 演算子を使う場合<br />
  `{{ 値 演算子 比較対象 }}`

        :::hugo
        {{ if .Language == "ja"}} ... {{ end }}

## 関数 / 演算子の一覧

|関数 | 演算子 |意味
|:--|:--|:--
|[eq](https://gohugo.io/functions/eq/) | =, == |値が同じであればtrue
|[ne](https://gohugo.io/functions/ne/) |!=, <> |値が異なればtrue
|[ge](https://gohugo.io/functions/ge/) |>= |値が大きい、もしくは同じであればtrue
|[gt](https://gohugo.io/functions/gt/) |> |値が大きければtrue
|[le](https://gohugo.io/functions/le/) |<= |値が小さい、もしくは同じであればtrue
|[lt](https://gohugo.io/functions/lt/) |< |値が小さければtrue
|[in](https://gohugo.io/functions/in/) |- |配列内に指定した値があればtrue
|not in |- |配列内に指定した値がなければtrue

## 配列
配列は [slice 関数](https://gohugo.io/functions/slice/)を使って定義します。
`{{ $配列名 := slice "値1" "値1" "値1" }}` のように使います。

    :::hugo
    {{/* arr = ["foo,"bar","buzz"] のイメージ */}}
    {{ $arr := slice "foo" "bar" "buzz" }}

配列内の要素を参照するには、[index 関数](https://gohugo.io/functions/index-function/)を使います。
`{{ index  $配列名 添字 }}` で取り出します。

    :::hugo
    {{ $arr := slice "foo" "bar" "buzz" }}
    <ul>
      <li>{{ index $arr 0 }}</li>
      <li>{{ index $arr 1 }}</li>
      <li>{{ index $arr 2 }}</li>
    </ul>

次のように出力されます。

    :::html
    <ul>
      <li>foo</li>
      <li>bar</li>
      <li>buzz</li>
    </ul>

ループ処理もできます。

    :::hugo
    {{ $arr := slice "foo" "bar" "buzz" }}
    <ul>
      {{ range $arr }}
        <li>{{ . }}</li>
      {{ end }}
    </ul>

## 繰り返し処理
繰り返し処理を行うには [range 関数](https://gohugo.io/functions/range/)を使います。

ループ内で取り出した要素は`{{ . }}`で参照しますが、次のように変数名を付けてアクセスもできます。

    :::hugo
    <ul>
      {{/* seq は連続した数字の配列をつくる関数 */}}
      {{ range $el := (seq 5) }}
        <li>{{ $el }}</li>
      {{ end }}
    </ul>

次のように出力されます。

    :::html
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
      <li>5</li>
    </ul>

添字も取り出せます。

    :::hugo
    <ul>
      {{/* seq は連続した数字の配列をつくる関数 */}}
      {{ range $index, $el := (seq 5) }}
        <li>{{ $index }}: {{ $el }}</li>
      {{ end }}
    </ul>

次のように出力されます。

    :::html
    <ul>
      <li>0: 1</li>
      <li>1: 2</li>
      <li>2: 3</li>
      <li>3: 4</li>
      <li>4: 5</li>
    </ul>
