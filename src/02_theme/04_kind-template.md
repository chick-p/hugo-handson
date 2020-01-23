# ページの種類を学ぶ

サイト構成に応じたページの種類についてです。

---

## 一般的なサイトのページ構成

一般的にサイトの構成は、次のような構成になっていることが多いです。

    :::bash
    トップページ # HUGO では「ホームページ」
    ├── カテゴリページ # HUGO では「セクション」
    │   ├── 記事ページ # HUGO では「シングルページ」
    │   └── 記事ページ
    └── カテゴリページ
        ├── 記事ページ
        └── 記事ページ

HUGO では「トップページ」「カテゴリページ」「記事ページ」といったページのタイプを、それぞれ「**ホームページ**」「**セクションページ**」「**シングルページ**」と呼びます。

HUGO で生成されるサイトでは、コンテンツのパス（ページの階層）によって、ページのタイプが決まります。

    :::bash
    content/
    ├── _index.md # ホームページ
    ├── page0-1.md # シングルページ
    ├── page0-2.md # シングルページ
    ├── section1/
    │   ├── _index.md # セクションページ
    │   ├── page1-1.md #シングルページ
    │   └── page1-2.md #シングルページ
    └── section2/
        ├── _index.md # セクションページ
        └── page2-1.md #シングルページ

HUGOではページのタイプに応じてテンプレートを定義できるため、先ほどの構成の場合、以下のようにテンプレートが適用されます。

    :::bash
    content/
    ├── _index.md # ホームページテンプレート
    ├── page0-1.md # シングルページテンプレート
    ├── page0-2.md # シングルページテンプレート
    ├── section1/
    │   ├── _index.md # セクションページテンプレート
    │   ├── page1-1.md #シングルページテンプレート
    │   └── page1-2.md #シングルページテンプレート
    └── section2/
        ├── _index.md # セクションページテンプレート
        └── page2-1.md #シングルページテンプレート

## テンプレートファイルが適用される優先順位
HUGO では、決まった順番でテンプレートファイルを探して、最初に見つかったファイルをテンプレートとして適用します（参考：[Hugo's Lookup Order](https://gohugo.io/templates/lookup-order/)）。

大まかにいうと、以下の順で適用するテンプレートを探します。

1. `layouts/` の下で、決められたファイル名のファイルを探す
2. `themes/<テーマ名>/layouts` の下で、決められたファイル名のファイルを探す

各テンプレートの優先順は次のとおりです。

### ホームページのテンプレート
ホームページテンプレートは、次のファイルのうち最初に見つかったファイルをテンプレートとして適用します。

    :::bash
    1. /layouts/index.html
    2. /layouts/_default/list.html
    3. /themes/<テーマ名>/layouts/index.html # 「テーマファイルを作る」で作った
    4. /themes/<テーマ名>/layouts/_default/list.html

### セクションページのテンプレート
セクションページテンプレートは、次のファイルのうち最初に見つかったファイルをテンプレートとして適用します。

    :::bash
    1. /layouts/section/<セクション名>.html
    2. /layouts/<セクション名>/list.html
    3. /layouts/_default/section.html
    4. /layouts/_default/list.html
    5. /themes/<テーマ名>/layouts/section/<セクション名>.html
    6. /themes/<テーマ名>/layouts/<セクション名>/list.html
    7. /themes/<テーマ名>/layouts/_default/section.html
    8. /themes/<テーマ名>/layouts/_default/list.html

`<セクション名>.html` を作れば、セクションごとに別々のテンプレートを利用できます。

### シングルページテンプレート
シングルページテンプレートは、次のファイルのうち最初に見つかったファイルをテンプレートとして適用します。

    :::bash
    1. /layouts/<タイプ名>/<レイアウト名>.html
    2. /layouts/<セクション名>/<レイアウト名>.html
    3. /layouts/<タイプ名>/single.html
    4. /layouts/<セクション名>/single.html
    5. /layouts/_default/single.html
    6. /themes/<テーマ名>/layouts/<タイプ名>/<レイアウト名>.html
    7. /themes/<テーマ名>/layouts/<セクション名>/<レイアウト名>.html
    8. /themes/<テーマ名>/layouts/<タイプ名>/single.html
    9. /themes/<テーマ名>/layouts/<セクション名>/single.html
    10. /themes/<テーマ名>/layouts/_default/single.html # 「テーマファイルを作る」で作った

## ポイント

サイトテンプレートを作る場合、以下のテンプレートファイルを用意するのがおすすめです。


|ページの種類 |テンプレートファイル
|:--|:--
|ホームページ |/layouts/index.html
|セクションページ |/layouts/_default/section.html
|シングルページ |/layouts/_default/single.html
