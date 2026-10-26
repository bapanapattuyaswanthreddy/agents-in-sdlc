---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで開発を速め、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- Issue の範囲を明確にすることの重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した請負業者は、ドキュメントを十分に整備していません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに同じカテゴリの別のゲームを表示し、ユーザーがゲームを探し続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い Issue がほかにあります。

どちらも優先度を下げられやすい作業であり、Copilot cloud agent に任せるのに適しています。エージェントに非同期で作業してもらう間、開発者は別のタスクに取り組み、後で戻って結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境を準備してタスクを開始します。実行中または完了後にセッションを確認できます。提案された解決策をレビューできる状態になると、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作にも細部に複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試しながら調整します。生成 AI を導入してもソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習で、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を済ませている場合は `.github/copilot-instructions.md` の効果を見たはずです。まだの場合は、この機会に確認してください。Copilot cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見られるようにします。

リポジトリの GitHub Web UI または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成されたプル リクエストに TSDoc コメント、TypeScript 規約、コメント ヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際には通常、環境の準備とセットアップ スクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアと同じように作業します。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に特別な[セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。学習を進めやすいよう、ラボでは事前に構成されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、マイグレーションとシードを済ませた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、ワークフローが成功するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く知られていますが、古い情報しかないプロジェクトや、ドキュメントが不足しているプロジェクトは少なくありません。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成が得意なので、cloud agent に依頼するのに適した作業です。バックグラウンドで必要なドキュメントを作成してもらい、次の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しい PR を開きます。PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した ToDo リストを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかる場合があります。Copilot は、前述のワークフローを実行して環境を作成し、プロジェクトを分析して、タスクへの取り組み方を検討しています。

13. リストと実行予定のタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターが表示されるまで、画面の更新が必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する過程を確認します。コードを調査して状態を把握し、計画を検討して決定し、コードを作成する様子が表示されます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。この機能を活用し、もう 1 つのタスクも cloud agent に割り当ててから、アプリに機能を追加するコード作成に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けの関連ゲーム欄の追加に時間がかからないとしても、その時間をほかの作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。
9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを確認できます。

![Copilot が Issue に取り組んでいることを目の絵文字で示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューします。次はカスタム エージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に処理できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

次の概念を学びました。

- コード生成環境のカスタマイズ
- 操作を安全に行うこと
- 範囲を明確にした Issue の重要性
- Copilot への Issue の割り当て

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
