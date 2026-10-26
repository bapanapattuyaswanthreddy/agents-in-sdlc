---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、これらの機能を明確な QA プロセスを定義するカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、この QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルに、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、これらの機能を Copilot がどう使うかを定義します。これらの機能を置き換えたり、別の会話が必要になったりするものではありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。プロファイルには、QA が実行するチェックと遵守する制約を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約をプロファイルに反映したか確認します。検証だけを依頼したときに、エージェントが機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、含めるとエージェントを識別しやすくなります。
3. プロファイルの指示を読み、要件を起点にして、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が証拠を示すこと、実装コードを変更する前に確認すること、変更をコミットしたりプル リクエストを開いたりしないことを確認します。
5. これらの責任や制約が不足している場合は、続ける前に既定のエージェントにプロファイルを修正させます。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。これにより、Issue と計画時の決定事項を引き継げます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後のエージェントが使用可能な一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了後にレポートを読みます。

## まとめと次のステップ

再利用できる専門的な役割をワークフローに加え、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューしました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートがレビュー可能な状態になりました。次は、機能 PR にまとめて Agent Merge を使います。[次のレッスンに進みます][next-lesson]。

## リソース

- [Copilot CLI 用カスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
