---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示とスキルの更新、品質保証 (QA) プロファイル、テストは 1 つのブランチに保存されています。まとめてレビューしてプル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを効率よく進めるため、フィルター機能と品質に関する作業全体を 1 つのセッションとブランチにまとめ、単一の PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能 PR 全体と検証の証拠を確認します。
- レビュー後に Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認権限を開発者が保持したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** を使うと、GitHub Copilot app でプル リクエストをマージするまでの残りの作業を自動化できます。有効にすると、アプリのセッションがプル リクエストを読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要なリベースなど、マージを妨げる問題を解決します。その後、GitHub が許可すると PR をマージします。バックグラウンドで動作し、アプリの再起動後も継続し、PR がマージされると自動的に停止します。

ここまでは自分で **Merge pull request** を選択しました。Agent Merge にその作業を任せることもできますが、コードの編集やマージには明示的な承認が必要です。マージ権限を許可する前に、Agent Merge が実行できる操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードを作成してレビューできたので、Agent Merge に PR のプロセスを管理させます。

1. エージェント ピッカーから **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンの表示が **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択して処理を開始します。ドロップダウンから項目を選ぶだけでは開始されません。

Agent Merge は次の処理を行います。

- タイトルと説明を付けてプル リクエストを作成します。
- Issue からセッションを開始した場合は、説明の本文で関連 Issue を参照します。
- ターゲット ブランチへのリベースや、マージ競合への対応を行います。
- CI を監視し、すべてのチェックが成功するようにします。
- 他の開発者や Copilot code review からのフィードバックがないか PR を監視し、コメントに対応する変更を加えます。
- すべてのチェックが成功した後、必要に応じて PR を自動的にマージします。

すべてのチェックが成功したら、Agent Merge に PR もマージさせます。

5. マージ権限を許可する前に、PR の差分と検証の証拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次のプロンプトで、マージを明示的に依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進捗を確認し、報告されたブロッカーを解決します。続行前に PR の状態が **Merged** であることを確認してください。マージ権限を有効にしたことやエージェントが作業中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge はリポジトリの保護や権限不足を回避しません。続行前にこれらのブロッカーを解決してください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発作業の複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能 PR 全体と検証の証拠を確認しました。
- レビュー後に Agent Merge を許可し、PR がマージされたことを確認しました。

次は、[既存のキャンバスを使ってトリアージ キャンバスを作成し][next-lesson]、エージェントと一緒に作業を調査、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
