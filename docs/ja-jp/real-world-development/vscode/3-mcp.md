---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使ってフィルター機能を構築しました。プル リクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動テストさせます。
- ブランチを公開して、フィルター機能のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外に接続できるとさらに強力になります。Copilot は Model Context Protocol (MCP) を使って、外部のツールやサービスと通信します。MCP は、エージェントが外部のツールやサービスとやり取りするための標準的な方法です。

![コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) とエージェント モードの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。リソースを通じて最新情報にアクセスし、ツールを使ってユーザーに代わる操作を実行できます。

ツールやリソースへのアクセスには、AI エージェントと外部のツールやサービスの通信を仲介する MCP サーバーを使います。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールとの通信を管理します。各サーバーは、AI エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーの例を示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。AI エージェントでリポジトリの作成や更新、Issue やプル リクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

さまざまなツールやリソースを利用できる MCP サーバーはほかにもあります。GitHub はエコシステムの発見と貢献を促す [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティ面では、MCP サーバーをほかの依存関係と同じように扱ってください。利用する前にソース コードを注意深く確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密情報や重要な操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot は Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるようになり、機能をテストできます。

1. `.vscode/mcp.json` で、`github` と並べて `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面に表示せずにブラウザーを実行するよう Playwright に指示します。デスクトップ表示のない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使うため、通常はブラウザーがインストール済みです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成を確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成を確認して信頼を選択し、起動を許可します。

手動での起動は省略できます。省略した場合は、次の手順で Copilot が初めてツールを使う際に VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、観察内容をまとめます。会話を離れずに機能を確認できる効率的な方法です。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目などのラベル付き構造要素を基に推論します。これは支援技術の動作と同じであり、簡単な機能確認をアクセシビリティの基本的なチェックにもできます。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ基準と照らし合わせます。想定と違う点があれば、質問するか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> テスト時には、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に起動してください。Copilot が Playwright MCP サーバーを初めて使う際、ブラウザーのダウンロードが必要な場合があります。不足が報告されたら `npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、PR を作成する方法はいくつかあります。GitHub Copilot を使っているので、関連 Issue を見つけて PR に関連付けてもらいましょう。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報取得や操作を実行できるようにします。GitHub ツールを初めて使うときに GitHub 認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前にブラウザーでフィルター機能を手動テストしました。
- ブランチを公開してフィルター機能のプル リクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなど、対象を絞った作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
