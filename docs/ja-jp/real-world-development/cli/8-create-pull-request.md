---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。まとめてレビューして、プル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを効率よく進めるため、フィルター機能と品質に関する作業全体を 1 つの会話とブランチにまとめ、単一の PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認権限を開発者が保持したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR のブロッカー (失敗した継続的インテグレーション (CI) チェックやレビュー コメントへの対応、必要なリベースなど) を処理し、リポジトリで利用できる場合は GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードを作成できたので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージが実行されます。
7. Copilot が「PR #14 was squash-merged successfully.」のようなメッセージを表示することを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可される方法でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発の複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
