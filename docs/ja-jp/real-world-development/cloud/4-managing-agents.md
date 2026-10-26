---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直近の 2 つの演習では、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつもあります。このミッション コントロールでは、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 実行中のセッションに指示を出して機能を追加依頼します。

## シナリオ

エージェントにハイコントラスト モードを依頼した後、チームはライト モードも追加する良い機会だと気づきました。サイトのスタイル変更と切り替え機能に取り組んでいるため、合わせて実装するのが合理的です。ハイコントラストに加えてライト モードも追加するよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインに表示されるタスク一覧を確認します。次のような、割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイコントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。次のような操作が含まれることがあります。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - MCP サーバーのセットアップ
   - 割り当てられたタスクを完了するための各種作業

> [!NOTE]
> 表示される手順は、Copilot の作業状況やアプローチによって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。PR と変更されたファイルを表示して、さらに詳しく監視できます。

## cloud agent に指示を出す

実行中のタスクを確認できたので、ハイコントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラスト モードを追加するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

   ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot の作業中にセッションを方向づけるダイアログを強調表示したエージェント ページの画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が完了すると、新しい依頼を作業に反映します。

これまでと同じく、完了まで数分かかります。その間、ここまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent の監視とガイドを行う中心的な場所である Copilot エージェント ページを確認しました。このミッション コントロールから、実行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクの範囲を調整したり追加のガイダンスを提供したりできます。

次の概念を学びました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視する
- 進行中のセッションに追加機能を依頼する

Copilot がアクセシビリティ機能を実装している間に、作成されたプル リクエストを[反復して改善する][next-lesson]方法を確認します。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
