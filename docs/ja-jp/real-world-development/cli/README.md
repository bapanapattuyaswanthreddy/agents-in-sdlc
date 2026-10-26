---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プル リクエストの自動化を学びながら、Tailspin Toys の変更を検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できます。コードベースの調査、コードの生成、コマンドの実行、外部ツールへの接続をすべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローを進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。
2. 星評価に対象を絞った変更を行い、ブラウザーでレビューして、最初のプル リクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から始め、Plan モードで方針を定め、Autopilot モードで構築してから、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト カバレッジ、最終検証の証拠を確認します。
8. フィルター機能全体の変更をレビューし、Agent Merge を使ってフィルター機能の PR をマージします。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使えるスラッシュ コマンドを確認します。
10. ワークフロー、再利用できるカスタマイズ、参考資料を振り返ります。

ワークショップでは、星評価と、指示・スキル・QA プロファイル・テストの更新を含むフィルター機能の 2 つの PR を作成します。フィルター機能と品質に関する作業は、同じ会話とブランチで続けるため、各ツールを学びながら作業を積み重ねられます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認します |
| [2. 星評価の追加: 小さく素早い改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を確認し、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使用する][ex5] | 繰り返し実行できるチェック | 既存スキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | CLI で MCP を構成し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使用する][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選択し、最終検証の証拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューして PR を作成し、Agent Merge を使用します |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加のリソースを確認します |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイしてサイトに接続します |

## 前提条件

ワークショップに参加する前に、次を用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI の使用を有効にしていることを確認してください。

## はじめに

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
