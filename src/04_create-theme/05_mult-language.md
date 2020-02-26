# サイトを多言語化する

HUGO には[Multilingual Mode](https://gohugo.io/content-management/multilingual/)という、多言語化機能があります。
この機能を使うと、`https://example.com/ja/` や `https://example.com/en/` のように、サイトのディレクトリパスにおうじて配信するコンテンツの言語を変更できます。

---

## コンテンツの多言語化

コンテンツを言語ごとに分けるには、2つの方法があります。

- ファイル名に言語コードを付けて分ける
    - example.**en**.md
    - example.**ja**.md
    - example.**zh**.md

- 言語コードのディレクトリでわける
    - content/**en**/example.md
    - content/**ja**/example.md
    - content/**zh**/example.md

今回はディレクトリで分ける方法で多言語化対応します。

### 設定ファイル

`config.toml` を開いて、多言語設定を追記します。

    :::toml hl_lines="2 3 10 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35"
    baseURL = "http://example.org/"
    # languageCode = "en-us"
    # title = "My New Hugo Site"
    theme = "mytheme"
    DefaultContentLanguage = "ja"
    defaultContentLanguageInSubdir= true

    [params]
    logo = "/img/logo.svg"
    # search_placeholder = "検索キーワードを入力"

    [languages]
    [languages.en]
    weight = 1
    title = "Hugo tutorial"
    LanguageName = "English"
    contentDir = "content/en"
    languageCode = "en-us"
    search_placeholder = "Enter search keywords"

    [languages.ja]
    weight = 2
    title = "Hugoチュートリアル"
    LanguageName = "日本語"
    contentDir = "content/ja"
    languageCode = "ja-jp"
    search_placeholder = "検索キーワードを入力"

    [languages.zh]
    weight = 3
    title = "Hugo教程"
    LanguageName = "中文(简体)"
    contentDir = "content/zh"
    languageCode = "zh-cn"
    search_placeholder = "输入搜索关键字"

!!! Note
    - `DefaultContentLanguage` で、`https://example.com/`（ディレクトリパスで言語を指定しない場合）で開いたときの言語を指定できます。
    - `[language.言語コード]` 以下にその言語に関する設定を記述します。
        - 各言語のコンテンツがあるディレクトリは `contentDir` で設定します。
        - サイト内に言語によって表示を切り替えたい文言がある場合（ここでは `search_placeholder`）も、この設定で言語に応じた文言を表示できます [^1]。

[^1]: 言語によって表示を切り替えたい文言が多い場合、`config.toml` に記述すると `config.toml` の内容が膨大になります。その場合は、[i18n 機能](https://gohugo.io/functions/i18n/) を使って、言語ごとの文言だけを別ファイルに切り出すこともできます。

### コンテンツの追加
以下のファイルを `content` と差し替えます。

### 変更前

    :::bash
    content
    ├── _index.md
    ├── docs
    │   ├── _index.md
    │   └── sample.md
    └── search_result.md

### 変更後

    :::bash
    content/
    ├── en
    │   ├── _index.md
    │   ├── docs
    │   │   ├── _index.md
    │   │   └── sample.md
    │   └── search_result.md
    ├── ja
    │   ├── _index.md
    │   ├── docs
    │   │   ├── _index.md
    │   │   └── sample.md
    │   └── search_result.md
    └── zh
        ├── _index.md
        ├── docs
        │   ├── _index.md
        │   └── sample.md
        └── search_result.md

### 動作確認

開発サーバを起動して確認してみましょう。
サブディレクトリの言語コードにおうじて、配信されるコンテンツの言語が切り替わります。

    :::bash
    # /Hugo/Sites/theme_tutorial 内
    $ hugo server

    # http://localhost:1313/ja/ , http://localhost:1313/en/ , http://localhost:1313/cn/ にアクセスして確認

## 言語選択メニュー

ヘッダーの言語選択メニューを作ります。

### テンプレートファイルの修正

`themes/mytheme/layouts/partials/header.html` を開いて、黄色の部分を修正します。

    :::hugo hl_lines="3 11 12 13 14 15 16"
    ...
        <nav class="search-box">
          <form action="{{ "/search_result" | relLangURL }}">
            <input type="text" name="q" aria-label="search" class="search-input" {{ with .Site.Params.search_placeholder }}placeholder="{{ . }}"{{ end }}>
            <input type="hidden" value="検索">
            <button type="submit" class="search-button"><img src="/img/search.svg" alt="search"></button>
          </form>
        </nav>
      </div>
      <nav class="language">
        <select name="language" onChange="location.href=value;">
          <option value="" selected>{{ .Site.Language.LanguageName }}</option>
          {{- range .Translations -}}
            <option value="{{ .RelPermalink }}">{{ .Site.Language.LanguageName }}</option>
          {{- end -}}
        </select>
      </nav>
    </div>

!!! Note
    - `relLangURL` でディレクトリパスの言語コードを加味した URL を利用できます。
    - `.Site.Language.LanguageName` は現在表示している言語の表示名です。表示名には、`config.toml` の `[language.言語コード]` の `LanguageName` で指定した値が使われています。
    - `.Translations` で現在表示している言語以外で、`config.toml` にある言語設定の一覧を取得します。
        - `.RelPermalink` は、ループで取り出した言語のコンテンツのパスです。
        - `.Site.Language.LanguageName` の `.` は、ループで取得した言語の情報を指しているので、ここでの `.Site.Language.LanguageName` は取り出した言語の表示名です。

### CSS の修正

`themes/mytheme/static/css/default.css` を開き、以下を追記します。

    :::css
    .language select {
      width: 100px;
      height: 35px;
      background-color: white;
      border: none;
      border-radius: 4px;
      line-height: 1.5rem;
    }

### 動作確認

開発サーバを起動して確認してみましょう。
ドロップダウンを変更すると、その言語のコンテンツが表示されます。

    :::bash
    # /Hugo/Sites/theme_tutorial 内
    $ hugo server

    # http://localhost:1313/ にアクセスして確認

