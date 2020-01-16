# Gitのインストールと設定

この節では、Git のインストールと設定を説明します。

---

## インストール
macOS の場合は、標準でインストールされています。

Windows の場合は、[git for windows](https://gitforwindows.org/)からインストーラーをダウンロードし、インストールします。

## ユーザー情報の設定
コミットに付与されるユーザー情報を設定します。初期設定では PC 名になっているので変更します。

### PC 全体で共通のユーザー情報を設定する場合

```bash
$ git config --global user.name "ユーザー名"
$ git config --global user.email "username@example.com"
```

### 特定のリポジトリだけユーザー情報を設定したい場合

```bash
$ git config --local user.name "ユーザー名"
$ git config --local user.email "username@example.com"
```
