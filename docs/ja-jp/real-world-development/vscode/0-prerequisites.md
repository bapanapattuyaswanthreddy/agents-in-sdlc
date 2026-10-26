---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が利用できることを確認します。

## ラボ リポジトリをセットアップする

演習で使うコードのコピーを作成するには、[テンプレート][template-repository]からインスタンスを作成します。新しいインスタンスにはラボに必要なファイルが含まれます。演習を進めながらこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講する場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリ テンプレートの設定を入力](../../../_images/ex0-repository-settings.png)

4. 後の演習で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動で作成されます。ワークショップではこれらの Issue を使います。自分で作成する必要はありません。

## Codespace を作成する

続いて、ラボの演習に使う codespace を作成します。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーから直接コードを記述、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応した高機能な IDE が用意されています。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい codespace を作成](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりも速く完了します。その間に GitHub Copilot のほかの機能を確認しておきましょう。次にそれらの機能を使います。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ここでは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらも、スムーズに演習を進められるよう必要な前提条件を備えています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使っている場合は、すでにインストールされているはずです。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、利用するには GitHub アカウントでの認証が必要な場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」などのメッセージを入力し、<kbd>Enter</kbd> を押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の手順に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ リポジトリのコピーを作成できました。また、コードを書き始めるときに使う codespace の作成を開始しました。

## 次のステップ

Copilot を使ってみましょう。プロジェクトの規約を Copilot に伝える[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
