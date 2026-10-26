---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は必要なファイルを探し、変更し、テストを実行して、問題をデバッグし、変更漏れがあればさらに修正します。作業は次々と増えていきます。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者と同じように、まず既存のプロジェクト構成を調査し、必要な更新を行い、テストなどを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は用途が異なります。エージェント モードは IDE 内で動作するため、すばやくフィードバックを得ながら対話できます。cloud agent はチーム メンバーのように非同期で作業し、Issue とプル リクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、連携した変更を行う。
- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー ヘルパーを追加しました。ここでは Copilot Agent Mode を使い、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲームをクラウドファンディングするプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは Drizzle ORM を通じて、フロントマターからローカルの SQLite データベースを直接照会します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能には標準 DOM API を使う範囲を限定した Astro `<script>` を使います。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では次の手順で実行できます。

1. codespace に戻ります。前の演習の後で `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. 次のコマンドで `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザー ウィンドウを表示するダイアログが出た場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。Codespace へのプライベート トンネルを通じて Web サーバーに接続します。

### Web サイトを確認する

Web サイトが起動したら、次の主な機能を確認できます。

- **ホーム ページ:** ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して拡張します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに Issue のバックログも自動作成されています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。**Agent** は変更を計画して複数ファイルに実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択欄](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、良好な体験を得やすいよう手順に含めています。ただし、LLM の動作は確率的なため、提案内容は手順の例と異なることがあります。これは正常であり、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同じように完了できます。

3. 次のプロンプトを送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに役立つと考える順に項目を優先付けします。

## 指示ファイルを確認する

エージェントにコード生成を依頼する前に、作業のコンテキストを提供する指示ファイルを確認します。Web サイトへの機能追加方法を説明する [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - 次のようなファイル タイプ別の指示ファイルへのリンク
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用したり、複雑な内容を小さなファイルに分けたり、参考となる例やテンプレートを Copilot に提示したりできます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルターの動作に関するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択欄](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを特定する様子を確認します。データ層のヘルパー、UI、テストを見つけて変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行います。タスクやコードベースの複雑さによっては数分かかります。処理中、コード エディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理の途中で一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーがなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys に機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して連携した変更を行う。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだ内容を活用し、ページネーションを実装するよう Copilot に依頼してみましょう。次の点を検討してください。

- 指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定値にするかを決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot にコンテキストを伝えたり修正を依頼したりして、反復する必要がある場合があります。Copilot を使う通常の作業方法です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 用途と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
