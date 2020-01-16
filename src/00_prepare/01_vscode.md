# VSCode のインストールと設定
この節では、Visual Studio Code のインストールとおすすめ設定を説明します。

---

## インストール

- [Visual Studio Code](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)（以下、VSCode）をインストールしてください。
- 日本語化が必要な場合は、「[Japanese Language Pack for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-ja)」という拡張をインストールしてください。

## HUGO 開発におすすめの設定

HUGO 開発におすすめの設定です。

設定する場合は、「設定内容」を VSCode の[全体の設定]または [ワークスペースの設定]の`setting.json` に記載してください。

```json
 // インデントのガイドラインを表示します
"editor.renderIndentGuides": true,
// タブのサイズをスペース2にします。
"editor.tabSize": 2,
 // 空白を･で表示します
"editor.renderWhitespace": "all",
// 制御文字を表示します
"editor.renderControlCharacters": true,
```

!!! Note
    全体の設定をする場合、次の手順で設定します。

    1. Ctrl(mac の場合は Command) + Shift + P でコマンドパレットを開きます。
    2. 「setting」と入力します。
    3. 「設定（JSON）を開く」を選択します。

## コンソールから VSCode を起動する

コンソールで VSCode を起動するには `code パス` コマンドを実行します。

パスに指定したディレクトリをワークスペースとして、VSCode を起動できます。左パネルにエキスプローラーが表示され、パス以下のファイルを一覧できます。

```bash
# ディレクトリを移動
$ cd ~/foo
# カレントディレクトリ（~/foo）をプロジェクトルートとして VSCode を起動する
$ code .
```

!!! Note
    mac の場合、`code` コマンドを実行するには設定が必要です

    1. Ctrl(mac の場合は Command) + Shift + P でコマンドパレットを開きます。
    2. 「Shell」と入力します。
    3. 「シェル コマンド：PATH 内に `code` コマンドをインストールします」を選択する

## おすすめの拡張
おすすめの VSCode 拡張です。

|拡張名 |説明
|:--|:--
|[Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer) |対になるカッコに色を付ける拡張です。
|[Auto Closing Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag) |自動で HTML の閉じタグを挿入する拡張です。
|[Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag) |開始タグを修正したときに、自動で対になる終了タグを修正する拡張です。
|[Highlight Matching Tag](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag)|タグにカーソルを合わせたときに対応するタグがハイライトされる拡張です。
|[HTML Snippets](https://marketplace.visualstudio.com/items?itemName=abusaidm.html-snippets) |シンタックスハイライトや HTML タグの入力補助をします。
|[IntelliSense for CSS class names in HTML](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion) |クラスやID属性の補完をします。
|[nbsp-vscode](https://marketplace.visualstudio.com/items?itemName=possan.nbsp-vscode) |`\&nbsp;` をハイライトします。
|[zenkaku](https://marketplace.visualstudio.com/items?itemName=mosapride.zenkaku) |全角スペースをハイライトします。
