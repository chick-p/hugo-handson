# カテゴリツリーの作成

カテゴリーツリーを作成します。

---

## カテゴリーツリーの作成

カテゴリーツリーには、現在開いているページ以下に所属する記事ページを表示します。

### テンプレートの修正

`themes/mytheme/layouts/partials/nav.html` を開いて、以下に書き換えます。

    :::hugo
    <nav class="nav">
      {{ template "navtree" (dict "section" .Site.Home "current" .) }}
      {{ define "navtree" }}
        {{ $section := .section }} {{/* 処理するセクション */}}
        {{ $current := .current }} {{/* 現在表示中のページ */}}
        <ul>
          {{ range $section.Sections }}
            <li><a href="{{ .RelPermalink }}">{{ .Title }}</a>
              {{ if .RegularPages }}
              <ul>
                {{ range .RegularPages}}
                <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
                {{ end }}
              </ul>
              {{ end }}
              {{ template "navtree" (dict "section" . "current" $current) }}
          {{ end }}
        </ul>
      {{ end }}
    </nav>

!!! Note
    - `dict` はキーと値のペアのマップを作る関数です。`(dict "section" .Site.Home "current" .)` とした場合、以下のマップが作られています。

            :::json
            {
              "section": サイト全体の情報,
              "current": 表示されているページの情報
            }

    - `{{ $section := .section }}` で、`$section` `section` キーで渡された情報がはいります。
    - `{{ range $section.Sections }}` で処理対象のセクション配下のセクションを取り出します。最初はサイト全体の情報が渡されているので、`content` 直下にあるセクションページが取り出されます。
    - `{{ if .RegularPages }}` で、取り出したセクションの中に記事ページがあれば、ループで取り出して表示します。
    - `{{ template "navtree" (dict "section" . "current" $current) }}` で、取り出しているセクションの中にさらにセクションが含まれていれば、テンプレートを再帰呼び出ししてその中の記事を表示します。


`themes/mytheme/static/css/default.css` に以下を追記します。

    :::css hl_lines="5 6 7 8 9 10 11 12 13"
    .nav {
      width: 300px;
    }

    .nav ul {
      padding: 0 12px;
    }

    .nav a {
      color: #000;
      line-height: 1.5;
      text-decoration: none;
    }


### 現在表示しているページをハイライトする

今開いているページをハイライトします。

`themes/mytheme/layouts/partials/nav.html` を開いて、黄色の部分を書き換えます。

    :::hugo hl_lines="8 12"
    <nav class="nav">
      {{ template "navtree" (dict "section" .Site.Home "current" .) }}
      {{ define "navtree" }}
        {{ $section := .section }} {{/* 処理するセクション */}}
        {{ $current := .current }} {{/* 現在表示中のページ */}}
        <ul>
          {{ range $section.Sections }}
            <li><a {{ if (eq . $current) }}class="current"{{ end }} href="{{ .RelPermalink }}">{{ .Title }}</a>
              {{ if .RegularPages }}
              <ul>
                {{ range .RegularPages}}
                <li><a {{ if (eq . $current) }}class="current"{{ end }} href="{{ .RelPermalink }}">{{ .Title }}</a></li>
                {{ end }}
              </ul>
              {{ end }}
              {{ template "navtree" (dict "section" . "current" $current) }}
          {{ end }}
        </ul>
      {{ end }}
    </nav>


!!! Note
    - 8行目は、表示されているページが、セクションページを想定した場合です。処理するセクション配下にあるセクションページと一致しているなら、背景色を付けます。
    - 12行目は、表示されているページが、シングルページを想定した場合です。処理するセクション配下にあるセクションページなら、背景色を付けます。

`themes/mytheme/static/css/default.css` に以下を追記します。

    ::css hl_lines="7 8 9"
    .nav a {
      color: #000;
      line-height: 1.5;
      text-decoration: none;
    }

    .nav a.current {
      background-color: #ddd;
    }

### 今開いているページのカテゴリだけを表示する

`themes/mytheme/layouts/partials/nav.html` を開いて、黄色の部分を書き換えます。

    :::hugo hl_lines="6 10"
    <nav class="nav">
      {{ template "navtree" (dict "section" .Site.Home "current" .) }}
      {{ define "navtree" }}
        {{ $section := .section }} {{/* 処理するセクション */}}
        {{ $current := .current }} {{/* 現在表示中のページ */}}
        <ul {{if not (or ($section.IsAncestor $current) ($section.IsHome)) }}class="close"{{end}}>
          {{ range $section.Sections }}
            <li><a {{ if (eq . $current) }}class="current"{{ end }} href="{{ .RelPermalink }}">{{ .Title }}</a>
              {{ if .RegularPages }}
              <ul {{if not (.IsAncestor $current) }}class="close"{{end}}>
                {{ range .RegularPages}}
                <li><a {{ if (eq . $current) }}class="current"{{ end }} href="{{ .RelPermalink }}">{{ .Title }}</a></li>
                {{ end }}
              </ul>
              {{ end }}
              {{ template "navtree" (dict "section" . "current" $current) }}
          {{ end }}
        </ul>
      {{ end }}
    </nav>

!!! Note
    - `.IsAncestor 比較対象` は、「比較対象が自分（`.`）の祖先だったら」という意味です。
    - 6行目の場合、「"処理しているセクション"が"表示されたページの祖先"」または「処理しているセクションが第一階層」ではない場合、配下の一覧（`<ul>`）を非表示にします。
    - 10行目の場合、「"処理しているセクション以下にあるセクション"が"表示されたページの祖先"」ではない場合、処理しているセクション以下の記事一覧（`<ul>`）を非表示にします。

`themes/mytheme/static/css/default.css` に以下を追記します。

    :::css hl_lines="5 6 7"
    .nav a.current {
      background-color: #ddd;
    }

    .nav .close {
      display: none;
    }
