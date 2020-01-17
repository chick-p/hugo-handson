# HUGO の開発環境の構築
この節では、HUGO のインストールなどの準備事項を説明します。

---

## HUGO のインストール
HUGO をインストールします。

### masOS の場合

1. Homebrew がない場合、[Homebrew](https://brew.sh/) をインストールします。

        :::bash
        $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

1. HUGO をインストールします。

        :::bash
        $ brew install hugo

1. 作業用ディレクトリを作成します。

        :::bash
        $ mkdir ~/Sites

1. 次のようなファイル構成になっていれば OK です。

        :::bash
        /User/username
        ├── ...
        ├── Sites
        ├── ...

### Windows の場合

1. Cドライブ直下に「Hugo」ディレクトリを作成します。

2. 「Hugo」ディレクトリの下に「bin」「Sites」ディレクトリを作成します。

3. [HUGO Releases](https://github.com/gohugoio/hugo/releases) の「hugo_xxx_Windows-64bit.zip」をダウンロードします。xxx はバージョンです。

4. ダウンロードした zip ファイルを解凍します。「hugo.exe」を 2. で作成した「bin」の下にコピーします。

5. 環境変数に「`C:\Hugo\bin`」を追加します。

6. 次のようなファイル構成になっていれば OK です。

        :::bash
        C:\
        ├── HUGO
        │   ├── bin # 実行ファイルディレクトリ
        │   │    └── hugo.exe
        │   └── Sites # 作業用ディレクトリ
        ├── ...

### インストールできたかの確認

1. 次のコマンドを実行します。

        :::bash
        $ hugo version

1. 次のように、バージョン番号が表示されれば OK です。

        :::bash
        $ hugo version
        Hugo Static Site Generator v0.62.2/extended darwin/amd64 BuildDate: unknown

## 参考

- [Hugo Install - mac](https://gohugo.io/getting-started/installing/#homebrew-macos)
- [Hugo Install - Windows: Technical User](https://gohugo.io/getting-started/installing/#windows)
