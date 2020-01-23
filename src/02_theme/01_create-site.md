# サンプルサイトとその構成
この章では、独自テーマを作っていきます。
まずは、テーマを作るにあたって、テーマを適用して確認するためのサンプルサイトを作ります。

---

## サイトの作成

1. コンソールで `/Hugo/Sites` に移動します。

        :::bash
        # Windows の場合
        $ cd /Hugo/Sites

        # mac の場合
        $ cd ~/Sites

1. 「theme_tutorial」という名前のサイトを作成します。

        :::bash
        $ hugo new site theme_tutorial

1. 「Sites」の下に「hugo_tutorial」が作成されていれば OK です。

## サイトディレクトリの構成
生成されたサイトのディレクトリ構成を見てみましょう。※がついているディレクトリが、今後触っていくディレクトリです。

参考：[Directory Structure](https://gohugo.io/getting-started/directory-structure/)

    :::bash
    theme_tutorial
    ├── archetypes
    ├── content※
    ├── data
    ├── layouts
    ├── resources
    ├── static※
    ├── themes※
    └── config.toml

### archetypes
ファイルのひな形を置くディレクトリです。`hugo new ファイル名.md` で記事ファイルを作ったときに、ひな形が利用されます。

### content※
記事ファイルを置くディレクトリです。`hugo new ファイル名.md` でファイルを作ると、このディレクトリ以下に記事ファイルが生成されます。

### data
テンプレートから参照するデータセットのファイルを置くディレクトリです。静的なデータセットが有る場合に、このディレクトリに置いて記事ページから参照できます。

### layouts
サイトテンプレートのレイアウトファイルを置くディレクトリです。
配布されたテーマをカスタマイズしたいとき、このディレクトリにテンプレートファイルを置くと、テーマのデザインを上書きできます。

### resources
HUGO が生成するリソースファイルが置かれるディレクトリです。
例えば、レイアウト調整に Sass を使った場合、生成された css などが置かれます。

### static※
記事以外の静的ファイル（画像など）を置くディレクトリです。

### themes※
HUGO のテーマファイルを置くディレクトリです。

### config.toml※
サイトの設定ファイルです。サイト全体に関わる設定はこのファイルに記述します。
