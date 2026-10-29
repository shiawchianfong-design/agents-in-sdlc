---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルをすべて探し、変更を加え、テストを実行し、問題をデバッグし、更新し忘れたファイルを見つけて、さらに変更を加えるという作業を繰り返します。作業は尽きません。

このようなときに Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業できるように設計されています。まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使って、Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに設計されています。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue やプル リクエストを通じて連携します。

この演習では、次の内容を学習します。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数ファイルにわたる変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリーとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。ここでは、Copilot Agent Mode を使って、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、その詳細を確認できます。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは Drizzle ORM を通じて、フロントマターからローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に配置し、インタラクティブな機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習を終えていれば `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバーアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> codespace を使用している場合、Codespace ターミナルの localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを実行している codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

Web サイトは動作していますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は複数のファイルを調べて変更を計画、実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の内容を比較的一貫させ、学習しやすくすることを意図しています。ただし、LLM は確率的に動作するため、ワークショップに記載されたものと異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使用する場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を基準に、優先順位も付けていることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトの機能追加に関するコンテキストが記載された [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使用します。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - 次のようなファイル形式に応じた指示ファイルへのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、複雑な内容を小さく管理しやすい単位に分け、例やテンプレートを提示できます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データ層 (`src/lib/`) に残りのフィルターロジックを追加または調整する。
- フィルター動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功する必要があります。Copilot Agent Mode にこれらの作業を依頼できます。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探す様子を確認します。データ層のヘルパーと UI、テストが見つかり、ファイルの変更とテストの実行が始まります。

   ![Copilot がプロジェクトファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを行います。タスクとコードベースの複雑さによっては、数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示されることがあります。Copilot が完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テストの実行を許可するか Copilot Chat ペインで確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に何度か停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクを完了し、エラーが見つからなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了したチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作とコードの確認が終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。次の内容を実践しました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数ファイルにわたる変更を行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は、[Playwright MCP server で機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えていくと、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼してみましょう。次の点を検討してください。

- 指示ファイルなど、既存のベストプラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、実装方法を検討する。
- 希望する実装に必要なガイダンスを Copilot に提示するプロンプトを作成する。
- GitHub Copilot に変更を依頼してコンテキストを追加しながら、繰り返し改善する。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: 各モードの機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
