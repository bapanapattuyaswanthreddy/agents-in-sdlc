---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクの進め方に指定がある場合や、Copilot が把握すべき背景情報がある場合は、その情報を参照できるようにしておきます。[指示ファイル][instruction-files]を使うと、Copilot は実行する内容だけでなく、その進め方も理解できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルによって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは指示の効果を比べる「変更前／変更後」のプロンプトは使いません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて比較するのが難しいからです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの冒頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるように必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定し、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイル タイプやタスクに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティス、全体に適用するガイダンスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
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
> Copilot に指示ファイルを生成してもらうこともできます。利用する環境によって方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当するレッスンで説明します。
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
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を再利用可能な小さなファイルに分けたり、コード生成時に参考にする例を Copilot に示したりできます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc コメントとファイル コメントのヘッダーに関する規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内のドキュメントとして TSDoc コメントとファイル レベルのコメント ヘッダーを追加するプロジェクト全体の規則です。後続の演習で Issue を割り当てると、cloud agent はこの規則を使います。

編集前に作業ブランチを準備します (cloud agent は Issue の対象ブランチにある指示を読み取りますが、ここでは変更を整理してコミットします)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい指示を使えるように、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチを省略して `main` に直接コミットできます。ここでは実際のプロジェクトに近い手順を体験するためにブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを読み取る仕組みを確認し、リポジトリ全体に適用される標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプル リクエストに、その標準が反映されます。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
