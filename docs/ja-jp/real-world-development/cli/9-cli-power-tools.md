---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意の cloud agent 委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、操作に使える多数のスラッシュ コマンドが用意されています。これらのコマンドから、高度な機能、内部の状態、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。このレッスンでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話コンテキストを管理する方法を確認します。
- `/model` を使って、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローを完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加の機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう分かれているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きく変化しないことがあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを自分で選びたい場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、代わりに `/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、使用するモデルを一覧から選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> キーを押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストールされているバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合う保存先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並行化または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分けて並行実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プル リクエストを作成することもある Copilot cloud agent にタスクを送信できます。

これらのコマンドは worktree やリモート作業を追加する場合があるため、任意の機能です。試す場合は、新しいスコープの明確なタスクから始め、通常のワークフローに沿って結果を確認してください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、構成やセッション共有、内部状態の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` でコンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。これまで学んだ内容と次のステップを[振り返り][next-lesson]、引き続き学習しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
