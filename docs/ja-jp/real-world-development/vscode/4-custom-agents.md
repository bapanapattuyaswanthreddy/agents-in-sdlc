---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベスト プラクティス、コーディング パターン、専門知識を提供します。チームの専門知識を再利用可能なエージェントとして共有できます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記載します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、概念上の重複があります。どちらも主に Markdown ファイルで定義され、AI に操作方法を伝えます。簡潔に分けると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを連携させます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビュー、更新します。その作業の一環として、プル リクエスト ワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI の活用と同じように、実際の環境やシナリオで試して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近のユーザー フィードバックによると、背景色と文字色のコントラストが不十分で、現在のダーク テーマを読みづらいと感じる人がいます。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。機能の生成にカスタム エージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのようにガイドするか、内容を確認します。

Codespace に戻り、ターミナルで `main` から新しいブランチに切り替えて、アクセシビリティ機能の作業を開始します (演習 3 のフィルター機能 PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴や間違い

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたカスタム エージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、その指示とツールを使うチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択欄](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。既定では現在のエージェント **default** が表示されています。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクト ファイルを編集します。編集内容はファイル パスと差分を含む形でチャットに表示されます。
6. 変更が加わると、**Files changed** の表示が更新され、このエージェントが変更したファイルを確認できます。

> [!NOTE]
> 処理には数分かかる場合があります。Copilot はリポジトリに実際の変更を加え、Astro コンポーネント、CSS、関連テストなどを編集します。

次の演習では、進行中の作業を確認し、エージェントに追加の指示を出します。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域に合わせた専門的な AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形でまとめ、Copilot が特定の種類の作業を効果的に進められます。

次の概念を学びました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は、[エージェントの作業を監視して方向づけます][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
