---
title: "演習 3 - カスタム エージェント"
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

カスタム エージェントを使って、次のことを学びます。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近のユーザー フィードバックによると、背景色と文字色のコントラストが不十分で、現在のダーク テーマを読みづらいと感じる人がいます。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。機能の生成にカスタム エージェントを使います。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのようにガイドするか、内容を確認します。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴や間違い

## Issue を作成して割り当てる

ミッション コントロールは、環境内のすべてのエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を出せます。まず、ハイコントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面で、カスタム エージェントと Accessibility を強調表示](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域に合わせた専門的な AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形でまとめ、Copilot が特定の種類の作業を効果的に進められます。

次の概念を学びました。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て

Copilot がハイコントラスト モードを実装している間に、ミッション コントロールから[エージェント セッションを監視して方向づける][next-lesson]方法を確認します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
