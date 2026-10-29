---
title: "レッスン 4 - カスタム指示による Copilot のガイド"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用するうえで、コンテキストは重要です。作業に特定の方法が求められる場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、作成したいコードだけでなく、その構成方法も記載できます。フィルター機能を実装したので、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示とパス指定の指示ファイルがエージェントにどのように伝わるかを確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載する。`db` 引数を注入する場合は、その点も含める。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントを最新の状態に保つ。
- 既存の書式設定と lint のガイダンスを維持する。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解できるように、コンテキストや設定を指定できます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定すると、より適切な提案やコードスニペットを得られます。リポジトリ全体に対する指示も、特定のファイルやタスクに対する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイル形式、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの指示ファイル形式への対応は環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

プロジェクトには、作業を始めるための指示ファイルがすでに含まれています。変更を加える前に、現在の内容と、その影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob で指示の対象ファイルを指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示は出発点として適切ですが、まだ不足があります。中核となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しい指示を読み込めるように、指示を保存した後で会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替えてから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパス指定の指示ファイルがエージェントにどのように伝わるかを確認した。
- コーディング標準に従うよう指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
