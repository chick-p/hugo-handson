
# テーマテンプレートの構成

作成したテーマテンプレートの構成について説明します。

---

## 構成
テーマのディレクトリの構成を見てみましょう。
※がついているディレクトリやファイルは、今後触っていくディレクトリ・ファイルです。

    :::bash
    themes/mytheme
    ├── LICENSE
    ├── archetypes
    │   └── default.md
    ├── layouts
    │   ├── 404.html
    │   ├── index.html
    │   ├── _default
    │   │   ├── baseof.html
    │   │   ├── list.html
    │   │   └── single.html
    │   └── partials
    │       ├── footer.html
    │       ├── head.html
    │       └── header.html
    ├── static
    │   ├── css
    │   └── js
    └── theme.toml

### LICENSE
テーマを配布するときのライセンスを定義します。

### archetypes
テーマ独自の記事ファイルのひな形を置くディレクトリです。

### layouts※
テーマのレイアウトファイルを置くディレクトリです。

#### layouts ディレクトリ直下

- 404.html※
    - 404 エラーページのテンプレートです。空のファイルが生成されます。
- index.html※
    - サイトのトップページのテンプレートです。

#### _default 以下

##### baseof.html※

- サイト全体のテンプレートです。
- このファイル内で、部品化した別のHTMLファイルで定義しているテンプレートを呼び出しています。

##### list.html※

- 記事の一覧ページなどのテンプレートです。
- 空のファイルが生成されます。

##### single.html※

- 記事ページのテンプレートです。空ページが生成されます。

#### partials 以下

##### footer.html※

- フッター部分を記述するため部品化されたテンプレートです。
- `baseof.html` から呼び出されています。

##### head.html※

- `<head>` 要素内を記述するため部品化されたテンプレートです。
- `baseof.html` から呼び出されています。

##### header.html※

- ヘッダー部分を記述するため部品化されたテンプレートです。
- `baseof.html` から呼び出されています。

### static※
レイアウトファイルから参照する静的ファイル（css / js）を置くディレクトリです。

### theme.toml
テーマを配布するときのテーマ設定を定義します。

## 
