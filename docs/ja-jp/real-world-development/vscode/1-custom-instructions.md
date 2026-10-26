---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクの進め方に指定がある場合や、Copilot が把握すべき背景情報がある場合は、その情報を参照できるようにしておきます。Copilot に特定のコンテキストを共有する方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成に関するガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルによって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示のまま、フィルター機能の最初のデータ部分 (パブリッシャー ヘルパー) を生成します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新済みのガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定したすべての標準に沿うとは限りません。Copilot の動作は確率的です。この演習の目的は、指示を更新した後の動作の**傾向**を確認することであり、出力を完全に一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの冒頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるように必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定し、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイル タイプやタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られます。技術スタック、開発対象の概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル タイプに対する指示です。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ単位の指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく扱うことは、このワークショップの範囲外です。ただし、サンプル プロジェクトの例は代表的な方法を示しています。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体のコーディング標準など、プロジェクト全体に関する情報を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層などのファイル タイプや、特定のタスク向けの指示を記載します。
- 自然な言葉で、明確にガイダンスを書きます。コードの良い例と避けるべき例を示すと効果的です。

指示ファイルの作り方に唯一の正解はありません。AI の使い方と同じように、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。環境によって方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、共通の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関する指針も確認してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの glob を指定し、指示が適用されるファイルを決めます。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分けたり、コード生成時に参考にする例を Copilot に示したりできます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc コメントとファイル コメントのヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルのままプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを参照できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択欄](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、ヘルパー ファイルやテストなどのコードを更新します。
6. 生成されたヘルパーは、最初の引数として `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数になっていることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 生成コードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメント コメントが追加されることがあります。その場合も問題ありません。指示を更新した後に一貫性が向上することが、この演習のポイントです。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供するファイルです。コードの提案を改善できるよう、コーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内ドキュメントの規則はまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示はリポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイル タイプ、タスクに適用する `*.instructions.md` のどちらにも作成できます。今回追加したドキュメント コメントの規則のようなプロジェクト全体の標準は、リポジトリ単位のファイルに記載します。

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot をガイドし、次の演習で利用するコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリのほかのファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで次を実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、プル リクエストを作成し、バックログのドキュメント標準に関する Issue に関連付けます。手動でも実行できますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せます。Issue を見つけて PR を作成し、マージするよう依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 操作を依頼します。プロジェクト テンプレートでは GitHub ツールがワークスペースに接続済みなので、セットアップは不要です。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従ってください。この接続の仕組み (Model Context Protocol) は後の演習で扱います。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択欄で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と、今後のフィルター作業に向けたパブリッシャー ヘルパーの基盤を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して適切な場合に選択します。
6. PR 作成後、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドの実行を Copilot に許可するよう求められたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを読み取る仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示のままコード生成プロンプトを送り、Copilot の出力を確認しました。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で作業を続けられるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は [エージェント モード][next-lesson]を使い、コードベース全体にまたがる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
