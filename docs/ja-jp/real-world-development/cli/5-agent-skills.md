---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも多くの作業があります。手動で動作を確認し、指示ファイルを使ってコーディング標準に従わせる方法を学びました。では、テストや lint、継続的インテグレーション (CI) に関するほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめ、必要に応じてエージェントが読み込みます。基本的には、スキル名のフォルダーに `SKILL.md` という名前の Markdown ファイルを置きます。Markdown ファイルにはスキルの名前と説明を定義するフロントマター、機能の概要、スキルを呼び出すタイミングのガイダンスを記載します。フォルダーには、スキルが実行時に使うスクリプトやほかのリソースを含むサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用できるリポジトリ資産としてプロジェクトの `.github/skills` フォルダーに置くか、ユーザーのスキル フォルダー `~/.copilot/skills` に置くことができます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明を基に、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の実行をどのようにガイドしているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

スキルは、Copilot CLI から直接、または自然言語で呼び出せます。Copilot に 3 つのチェックを実行するよう依頼します。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を確認できるレポートに改善します。スキルにレポート形式の指示を追加します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に、次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、動作を確認します。Copilot CLI は会話を再起動せずに、編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを読み、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は機能 PR のフィルター機能と一緒に含まれます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルの例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
