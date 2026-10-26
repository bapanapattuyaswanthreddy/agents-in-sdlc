---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルター機能の Issue を見つけました。
- 星評価を追加し、ポート フォワードしたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターの動作を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能全体の変更をレビューし、Agent Merge によるフィルター機能の PR マージを承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには 2 つの PR の節目があり、それぞれ更新済みの `main` から作成した別々のブランチで作業しました。

1. **星評価:** ゲーム カードに既存の `starRating` と、評価がない状態を明示して表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能の実装に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、これらの変更を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは、PR の前に要件とテスト カバレッジをレビューし、これらの結果をまとめました。

追加するテストは実際の不足を補う必要があります。QA の実行後に新しいテストが不要な場合もあります。マージを承認する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、その作業に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。構築前に方針を検討するには **Plan**、対象を絞った変更を一緒に進めるには **Interactive**、範囲の明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

インフラストラクチャと同様に、コンテキストも重要です。何を、なぜ、どのように構築したいかを明確に説明すると、結果が大きく変わります。

## さらに試す

中心的なワークフローは完了しました。次の CLI 機能も試してみてください。

- `/review` を使って、コードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` を使って、問題を話し合い、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並行して進めます。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。製品コード、趣味のコード、長年作りたいと思っていた小さなアプリなどに活用してください。チームと学んだことを共有し、メンバーの経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
