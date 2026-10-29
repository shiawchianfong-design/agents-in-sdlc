---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。作業に特定の方法が求められる場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、作業方法も伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパス指定の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更の前後でプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで影響を並べて示すのが難しいためです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ層には必ずユニットテストを用意する。
- UI はダークモードにして、モダンな印象にする。
- TSDoc のドキュメントコメントをコードに追加する。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加する。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解できるように、コンテキストや設定を指定できます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定すると、より適切な提案やコードスニペットを得られます。リポジトリ全体に対する指示も、特定のファイルやタスクに対する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、多くのチャットや CLI のリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用できます。TypeScript などの言語や、UI コンポーネントの作成、ユニットテストの追加などのタスクに関する指示を指定できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく扱うことはこのワークショップの範囲外ですが、サンプルプロジェクトにあるファイルから一般的な方法を確認できます。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、作成するものやプロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` ファイルには、ユニットテストや Astro コンポーネント、データ層などのファイル形式や、特定のタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作成方法は1つだけではありません。AI と同様、実際に試して改善しながら、プロジェクトやシナリオに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。操作方法は環境によって異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。該当する場合は、利用中の環境に合わせたレッスンで手順を説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェントなどが集められたリポジトリ [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。基本となる `copilot-instructions.md` が1つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針があります。これらの指示は、Copilot を使ったあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対パスで対象ファイルを指定する glob です。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示す方法を確認できます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントをまだ必須にしていません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーに関するルールを追加します。

## リポジトリに新しい標準を追加する

ここで行う編集は1か所だけです。コード内のドキュメントとして TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを追加する、プロジェクト全体のルールを設定します。後続の演習で Issue を割り当てると、cloud agent はこの指示に従います。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは変更をきれいにコミットします。

1. codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` には Copilot が参照するプロジェクトレベルの情報を記載します。リポジトリのコーディング標準にドキュメントのルールを加えて、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。ここではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直下に、次の Markdown を追加します。TSDoc のドキュメントコメントとファイルコメントヘッダーを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル形式、タスク向けに `*.instructions.md` として作成することもできます。ドキュメントコメントのルールのようにプロジェクト全体に適用する標準は、リポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。cloud agent が指示を利用できるように、変更を `main` に反映してください。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成してマージします。次の演習で Issue に割り当てる際、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` へ直接コミットできます。ここでは実際のプロジェクトでの作業方法を体験できるよう、ブランチを作成しています。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を追加しました。このハーネスの残りで cloud agent が作成するプル リクエストをレビューするときに、この標準がどのように適用されたかを確認します。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
