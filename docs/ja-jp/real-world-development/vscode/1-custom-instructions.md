---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。作業に特定の方法が求められる場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成に関するガイダンスを Copilot に提供できます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパス指定の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認する。
- 現在の指示を使って、フィルター機能用の最初のデータスライス (publishers ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認する。
- 指示の更新とフィルター機能の一部を `main` にコミットし、Copilot が次の演習で更新後のガイダンスを使えるようにする。

> [!CAUTION]
> 生成されたコードが設定した標準に従わない場合があります。Copilot は確率的に動作します。この演習では、指示を更新した後の動作の**傾向**を確認します。出力が文字単位で同じになることを期待するものではありません。

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

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の**一貫性**の向上を確認することが目的です。

## リポジトリに新しい標準を追加する

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

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が対象ファイルを参照するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用するよう指示し、次の演習で使用する実際のコードに反映できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示ファイルとヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示ファイルの更新とヘルパーの基盤に関する Copilot の変更をすべてステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプル リクエストを作成します。手動で行うこともできますが、プロジェクトに接続されている GitHub ツールを使って Copilot に依頼できます。Issue を検索し、Issue をクローズする PR を作成してからマージするように依頼します。

> [!NOTE]
> これは Copilot が GitHub 上で操作する最初の演習です。プロジェクトテンプレートでは、GitHub ツールがすでにワークスペースに接続されています。セットアップは必要ありません。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って認証してください。後の演習では、この接続の仕組み (Model Context Protocol) について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. Copilot に、指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう依頼します。指示の更新と新しい publishers ヘルパーの基盤の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探し、PR を作成します。
5. Copilot が代わりに GitHub 操作を実行する許可を求めたら、コマンドを確認して、適切であれば **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が代わりに GitHub 操作やシェルコマンドを実行する許可を求めたら、コマンドを確認して、適切であれば **Allow** を選択します。

GitHub Copilot の支援を使って、プル リクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- コード生成プロンプトを送り、*既存の*指示で Copilot が生成する内容を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加した。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認した。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使用できるようにした。

次は、[エージェントモード][next-lesson]を使って、Copilot にコードベース全体にわたる新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
