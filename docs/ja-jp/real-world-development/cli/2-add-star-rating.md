---
title: "レッスン 2 - 星評価を追加する: 小さく素早い改善"
description: "Copilot CLI でゲーム カードを小さく変更し、ポート フォワードしたブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに初めて変更を加えます。変更は小さくします。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、対象を絞った Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、ポート フォワードしたブラウザーで確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリ、パブリッシャー、説明しかありません。最初の練習として、エージェントに各カードの既存の評価を表示してもらいます。範囲が小さく完結しているため、最初のセッションに適しています。

## 会話の仕組み

**会話**では、タスクについて Copilot CLI と作業します。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作成せず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話を後で再開できますが、ファイルとブランチはディスク上の通常の Git 状態です。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細表示に使うファイルを特定して、必要なコードを作成します。Copilot CLI で新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューしてください。ここでは Copilot CLI 内で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストが両方の状態を対象にしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> すべての生成 AI ツールと同じく、Copilot の動作は確率的です。実際のコードは異なる場合があります。特定の実装ではなく、期待する動作になっているかを確認してください。

## 変更を確認する

コードを読むだけで、正しく動くと決めつけてはいけません。Copilot に Web サイトを起動してもらい、Codespaces でポート フォワードされたブラウザーから更新後の UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトに取り込むため、プル リクエスト (PR) を作成します。

1. 既定のエージェントに変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR の作成時に Copilot CLI が既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択し、PR を開きます。
4. 変更ファイルとチェックを確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って初めての変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで対象を絞った Copilot の会話を開始しました。
- ゲーム カードへの小さな変更をエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを起動し、ポート フォワードしたブラウザーで星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は、[フィルター機能の Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
