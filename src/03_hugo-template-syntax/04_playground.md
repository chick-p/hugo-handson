# HUGO の関数・変数を使って、サイトあるあるを作ってみる

---

## 次のページ、前のページへのリンクを表示する

前のページや次のページがあれば、そのページへのリンクを表示します。

■に当てはまる変数・関数を [HUGO ドキュメント](https://gohugo.io/documentation/)から探してみましょう。

    :::hugo
    {{ ■ .PrevInSection }}
      <a href="{{ ■ }}">＜ 前のページへ</a>
    {{ end }}
    {{ ■ .NextInSection }}
      <a href="{{ ■ }}">次のページへ ＞</a>
    {{ end }}

??? "解答"

        :::hugo
        {{ with .PrevInSection }}
          <a href="{{ .RelPermalink }}">＜ 前のページへ</a>
        {{ end }}
        {{ with .NextInSection }}
          <a href="{{ .RelPermalink }}"></a>次のページへ ＞</a>

    !!! Note
        - 「...があれば」なので `with` を使います。
        - そのページからの相対リンクを取得するには、`.RelPermalink` を使います。

<br />

## セクション内のページ一覧を表示する

表示しているページ以下にあるセクション（カテゴリ）の記事ページの一覧を表示します。

■に当てはまる変数・関数を [HUGO ドキュメント](https://gohugo.io/documentation/)から探してみましょう。

    :::hugo
    <h3>このカテゴリー以下の記事一覧</h3>
    <ul>
      {{ ■ ■ }}
        <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
      {{ end }}
    </ul>

??? "解答"

        :::hugo
        <h3>このカテゴリー以下の記事一覧</h3>
        <ul>
          {{ range .Pages }}
            <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
          {{ end }}
        </ul>


    !!! Note
        - 記事一覧は `.Pages` で取得できます。
        - リストなので `range` で回します。


    !!! Note
        ページの一覧は、デフォルトでは次の順にソートされます。

        Weight > Date > LinkTitle > FilePath

        次のように書くと、並び順を指定できます

        - Pages.ByWight
        - Pages.ByDate

    !!! Note
        `.Pages` は、記事ページの一覧とセクションページを取得します。カテゴリの記事ページのみ取得したい場合は `.RegularPages` を使います。

        - `.Pages` 記事ページの一覧とセクションページ
        - `.RegularPages` 記事ページの一覧
        - `.Sections` セクションページの一覧

<br />

## 同一セクションのページ一覧を表示する
表示しているページと同一セクション内のページ一覧を表示します。

■に当てはまる変数・関数を [HUGO ドキュメント](https://gohugo.io/documentation/)から探してみましょう。

    :::hugo
    <h3>このカテゴリーの記事一覧</h3>
    <ul>
      {{ ■ ■ }}
        <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
      {{ end }}
    </ul>

??? "解答"

        :::hugo
        <h3>このカテゴリーの記事一覧</h3>
        <ul>
          {{ range .CurrentSection.Pages }}
            <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
          {{ end }}
        </ul>

    !!! Note
        - 現在表示している記事が所属するセクションの記事は `.CurrentSection.Pages` で取得します。

<br />

## 最近作成した記事を表示する

サイト内の記事を作成日 (frontmatter の `date` の値) 順に表示してみましょう。

■に当てはまる変数・関数を [HUGO ドキュメント](https://gohugo.io/documentation/)から探してみましょう。

    :::hugo
    <h2>新着記事</h2>
    <ul>
      {{ range first 5 .Site.RegularPages.■.Reverse }}
        <li>
          <b><a href="{{ .RelPermalink }}">{{ .Title }}</a></b>
          <time>{{ .Date.Format "2006-01-02" }}</time>
        </li>
      {{ end }}
    </ul>

??? "解答"

        :::hugo
        <h2>新着記事</h2>
        <ul>
          {{ range first 5 .Site.RegularPages.ByDate.Reverse }}
            <li>
              <b><a href="{{ .RelPermalink }}">{{ .Title }}</a></b>
              <time>{{ .Date.Format "2006-01-02" }}</time>
            </li>
          {{ end }}
        </ul>

    !!! Note
        記事一覧は `.Site.RegularPages` で取得できます（参照: [List Of Content in Hugo](https://gohugo.io/templates/lists/)）


    !!! Note
        - `Site.RegularPages.ByLastmod.Reverse` とすると、最終更新日 (lastmod) 順にも並び替えできます。
        - frontmatter に `lastmod` が定義されていない場合は、`date` の値が利用されます。


<br />


## 記事の目次を表示する

記事内の見出しを使って目次を出力します。

■に当てはまる変数・関数を [HUGO ドキュメント](https://gohugo.io/documentation/)から探してみましょう。

    :::hugo
    <h2>目次</h2>
    {{ ■ }}

??? "解答"

        :::hugo
        <h2>目次</h2>
        {{ .TableOfContents }}

    !!! Note
        記事内の目次は `.TableOfContents` で取得します。

<br />

## サイト内の全ページ一覧を階層表示する
サイト内の全ページをツリー状に出力してみましょう。

以下のコードをホームページ用のテンプレート（`layouts/index.html`）に適用します。

    :::hugo
    {{- define "hierarchy" }}
      <ul>
        <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
          {{/* カレントセクション直下のセクションは再帰処理 */}}
          {{- range .Sections }}
            {{- template "hierarchy" . }}
          {{- end }}
          {{- /* カレントセクション直下の記事ページ */}}
          <ul>
            {{- range .RegularPages }}
              <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
            {{- end }}
          </ul>
        </li>
      </ul>
    {{- end }}

    <h2>全ページのリスト</h2>
    {{ template "hierarchy" .Site.Home }}

このコードでは、部分テンプレート `hierarchy` を定義し、`.Site.Home` をルートに設定して `hierarchy` を再帰的に呼び出しています。
`.Site.Home` はホームページオブジェクトで、サイト内のページ情報が入っています。

!!! Note
    - 1回目の呼び出し
        - `.Site.Home` のタイトル＝サイトタイトルを出力（3行目）
        - セクションがあれば、そのセクションページをルートに設定して `hierarchy` を呼び出し（5-7行目）
        - 同一階層に記事があれば記事タイトルを出力（10-12行目）
    - 2回目の呼び出し（2階層目）
        - 2階層目にあたるセクションページのタイトルを出力
        - セクションがあればそのセクションページをルートに設定して `hierarchy` を呼び出し（5-7行目）
          - 同一階層に記事があれば記事タイトルを出力（10-12行目）

下記のような階層構造のとき、

    :::bash
    content
    ├── section1
    │   ├── _index.md
    │   ├── section1-1
    │   │   ├── _index.md
    │   │   └── section1-1_article.md
    │   └── section1_artcle.md
    └── section2
        ├── _index.md
        └── section2-1
            ├── _index.md
            └── sample.md

出力されるイメージは次のとおりです。

**全ページのリスト**

- HomePage
  - Section1 SectionPage
    - Section1-1 SectionPage
      - Section1-1 の ArticlePage
    - Section1 の ArticlePage
  - Section2 SectionPage
    - Section2-1 SectionPage
      - Section2-1 の ArticlePage
