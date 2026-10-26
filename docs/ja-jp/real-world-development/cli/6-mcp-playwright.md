---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、多くの作業があります。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要もあります。MCP サーバーを使うと、Copilot CLI に組み込まれた機能を拡張し、追加のツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには実際に操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにして、変更の自動検証を進め、更新が期待どおりに動く確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムに通信できます。その結果、最新情報にアクセスし、ユーザーに代わって操作を実行できます。

ツールやリソースへのアクセスには、AI エージェントと外部のツールやサービスを仲介する MCP サーバーを使います。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーの例を示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステムの発見と貢献を促す [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同じように扱ってください。利用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Playwright MCP が Copilot CLI にすでに構成されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

先ほど、機能が期待どおりに動作することを手動で確認しました。次は Copilot が利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動で判断するため、明示的に指定する必要はありません。ただし、使用してほしいサーバーが分かっている場合はプロンプトに含めると、より一貫した結果を得やすくなり、トークンも節約できます。

2. Copilot がブラウザーで動作を確認するために実行する手順を確認します。
3. レポートを読み、すべて期待どおりに動作していることを確かめます。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- まだ利用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザー ツールを専門的な役割にまとめる [QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
