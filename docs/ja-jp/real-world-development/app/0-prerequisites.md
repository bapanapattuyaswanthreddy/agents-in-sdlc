---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の両方を一元的に扱うデスクトップアプリです。Issue やプル リクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、Astro で構築された Tailspin Toys アプリを GitHub Copilot app で更新しながら、ローカルで作業します。始める前に、Node.js がローカルにインストールされていることを確認し、Copilot app をインストールします。

このレッスンでは、次のことを行います。

- プロジェクトのテストをローカルで実行できるよう、Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を作成させ、Tailspin Toys のテスト スイートをローカルで実行します。そのため、プロジェクトに必要な唯一のランタイムである **[Node.js][nodejs]** が必要です。現在の **LTS** リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナル、または普段使用しているターミナルを開きます。
2. 次のコマンドを実行し、インストールされている Node.js と npm のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. 両方のコマンドが成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしていれば、次のセクションに進みます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、以降の手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストールが完了したら、新しいターミナル ウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールした Node.js と npm のバージョンが表示されることを確認します。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段のターミナルで成功しても、アプリで利用できるとは限りません。Node.js のインストールや PATH の変更後は、アプリを完全に終了してから再起動してください。セッションでいずれかのコマンドがまだ失敗する場合は、続ける前に[セッション環境のトラブルシューティング][session-environment]を確認してください。

> [!IMPORTANT]
> 各 worktree には、プロジェクトの依存関係と E2E チェック用の Playwright Chromium も必要です。worktree の準備では Tailspin Toys リポジトリの README に従い、インストールの要求は内容を確認してから承認してください。

## ラボ用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレート リポジトリ][template-repository]からコピーを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれています。アプリをインストールするときに接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![ドロップダウンから Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講する場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![テンプレートとして github-samples/tailspin-toys を指定し、リポジトリ名を入力した Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 後の演習で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこれらの Issue を使います。自分で作成する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、quality-checks スキル、既存のキャンバス拡張機能が含まれています。ワークショップ中にスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルが含まれているか進行役に確認してください。

## まとめと次のステップ

準備が整いました。このレッスンでは、次のことを行いました。

- プロジェクトをローカルでビルドしてテストできるよう、Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は、[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js のダウンロード][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
