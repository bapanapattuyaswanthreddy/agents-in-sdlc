---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コマンド ラインからコードベースを調査し、コードを生成し、コマンドを実行し、外部ツールを利用できます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずツールをインストールします。幸い、使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーでフィルター機能の Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI はその機能を、多くの開発者が日常的に作業するターミナルで利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. プロンプトが表示されたら、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 使い方を確認する

通常のシェル プロンプトで実行するコマンドは Codespace で直接実行されます。Copilot CLI の起動後は、自然言語がエージェントへの入力になり、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールされているバージョンで利用可能なコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 正しく動作していることを確認するため、Copilot に簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答の前にリポジトリを調査する様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルター機能の Issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。小さな変更を 1 つ完了した後、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチで動作し、自動的に worktree を作成しません。変更を加える前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みのアクセス許可の範囲外でツールを使う前に確認を求めます。このワークショップでは、`--yolo` を指定して再起動します。これはユーザーが承認したショートカットで、Codespace 内での確認プロンプトを省略し、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は `--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を有効にし、すべてのアクセス許可を自動で与えます。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定の設定としてエイリアスに登録しないでください。詳しくは[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、後のレッスンで Issue やプル リクエストを操作するため、`--enable-all-github-mcp-tools` を指定して GitHub MCP の読み書きツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには引き続きアクセスできます。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一つ簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして GitHub アカウントで認証し、ワークショップのリポジトリを信頼して、簡単な会話を試しました。また、組み込みの GitHub MCP サーバーでフィルター機能の Issue を見つけました。

次は、[最初の対象を絞った変更][next-lesson]に取り組み、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
