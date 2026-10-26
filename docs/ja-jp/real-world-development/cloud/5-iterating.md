---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加のため、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザイン チームが改善を重ねるための関連ゲーム機能、ハイコントラストとライト モードの切り替えを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックして改善します。

### シナリオ

これまで何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。そこで、チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視なしでタスクを実行するため、安全を保つためにセキュリティ上の制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権はコード作業に使うブランチに**限定**されます。
- cloud agent は GitHub Actions 内で動作し、独立した一時的な環境を作成して作業します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、Copilot cloud agent が作成したコード ドキュメントの PR を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールによってブロックされたという警告が表示されることがあります。既定では MCP サーバーなどの外部リソースへのアクセスが制限されているため、これは**想定された動作**です。必要に応じて [Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合は、PR に **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、ここまで学んだ内容を振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントなど、更新されたコードを確認します。具体的な変更内容は異なる場合があります。

   変更を確認するときは、演習 2 の冒頭で確認したカスタム指示ファイルに基づき、TSDoc コメント、TypeScript 規約、コメント ヘッダーが反映されているかを確認します。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローがキューに入ることを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確率的なため、常に同じ結果になる具体的な文面はありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. コードベースに追加したいドキュメントについて、上記の例または別の提案を使い、通常のユーザーと同じように **@copilot** を付けて変更を依頼するコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。更新のために新しいセッションが開始されることに注目してください。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得するまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローがキューに入り、単一の Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲーム数の調整や、コメント ヘッダーと TSDoc コメントの追加を依頼できます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると Copilot が確認し、新しいセッションで対応します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 でミッション コントロールから追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合は、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 特に次の点に注目して実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定の保存に使う local storage
   - ハイコントラスト モードとライト モードに関する CSS やスタイルの変更
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローがキューに入り、単一の Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーでハイコントラストとライト モードの切り替えを試します。ページの再読み込み後も設定が維持されることを確認します。

アクセシビリティ カスタム エージェントによって、アクセシビリティのベスト プラクティスに沿って機能が実装されました。懸念や改善点がある場合は、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — さらに作業を委任する

cloud agent に実際のバックログ項目を任せ、その間に別の作業を進めると効果的です。習慣づけるために、リポジトリにいくつか Issue を作成して Copilot に割り当ててみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力にどう反映されるかを確かめました。
- Copilot cloud agent に Issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ カスタム エージェントを作成して使用し、ハイコントラストとライト モードの切り替えを追加しました。
- Copilot エージェント ページをミッション コントロールとして使い、アクセシビリティ セッションを監視して方向づけました。
- `@copilot` に変更を依頼し、ワークフローを承認して、cloud agent のプル リクエストをレビューし反復しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学ぶ場合は、ほかのハーネスで今回の内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のことも試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の領域を担当するカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
