---
title: "レッスン 2 - 星評価を追加する: 小さく素早い改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を学びます。
- エージェントに小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリ、パブリッシャー、説明しかありません。最初の練習として、エージェントに各カードの既存の評価を表示してもらいます。小さく完結した変更なので、最初のセッションに適しています。

## セッションの構造

**セッション**とは、分離された独自のワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられるため、あるセッションで機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにグループ化されてサイドバーに表示され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調査して編集するときのエージェントの**ツール操作**、差分を含む**変更ファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

プロジェクトの調査と機能の実装を始めるため、新しいセッションを開始します。[アプリのセットアップ][prior-lesson]で GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用の新しいセッションを作成して、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択します。次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押してプロンプトを Copilot に送信します。

Copilot app は、まずプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調査し、新機能に必要なファイルを見つけてコードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものであってもマージ前にレビューしてください。Copilot app 内で変更を確認します。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同じく確率的に動作するため、コードが上記と完全に一致するとは限りません。ただし、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と `null` の場合の代替表示をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動してもらい、更新された UI を確認します。次のプロンプトでアプリを起動し、ブラウザー キャンバスでページを開くよう依頼します。

> [!TIP]
> キャンバスは Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを確認して自分でも作成しますが、ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトを送信します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価済みのゲーム カードに 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトでこのセッションの開発サーバーを停止します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。次は、コードベースに新しいコードをマージするためのプル リクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより PR 作成前に関連 Issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューパネルで PR を開きます。ここで必要に応じて内容をレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、プル リクエストをマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェント セッションを開始し、セッションの構成を学びました。
- ゲーム カードに小さく対象を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
