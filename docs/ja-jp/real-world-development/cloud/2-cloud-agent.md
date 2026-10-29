---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。エージェントは作業を完了すると、人間がレビューできるドラフト PR を作成します。時間のかかるタスクを任せることで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学習します。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- Issue の範囲を明確にすることの重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した委託業者は、ドキュメントを十分に作成していませんでした。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する別のゲームを表示して、ユーザーが引き続きゲームを見られるようにします。現時点では、UX の受け入れテストに使える簡単な実装で十分です。この作業はブロッカーですが、ほかに優先度の高い Issue があります。

このようなタスクは優先度が下がりやすいため、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent は非同期に作業できるので、開発者は別の作業に集中し、あとから Copilot の成果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てられると、Copilot は進捗を記録するドラフトプル リクエストを作成し、環境をセットアップしてタスクに取りかかります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備が整うと、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。Copilot cloud agent にタスクの範囲を指定する際は、[ベストプラクティス][cloud-agent-best-practices]を意識してください。AI ペアプログラマーとして捉え、段階的に進めながら、学習、実験、改善を繰り返します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とその役割を学びました。カスタム指示を実際に使ったことがあれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み、後でその影響を確認できるようにします。

リポジトリの GitHub Web UI で次のファイルを開きます。codespace を起動済みの場合は、そちらで開いてもかまいません。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc のドキュメントコメントに関する指示に注目してください。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` に一致するファイルを対象にします。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てる際、生成されたプル リクエストに TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。これらの指示ファイルの影響は、後の演習で PR をレビューするときにもう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードの作成には通常、特定の環境やセットアップスクリプトが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェアエンジニアのように作業するためです。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` ファイルで[特別なセットアップワークフロー][setup-workflow]を作成すると、作業を開始する前に開発ツールや依存関係をインストールできます。ラボの進行と学習のため、このワークフローはあらかじめ設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルに似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これにより、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメント作成が得意なので、cloud agent に依頼するタスクとして適しています。バックグラウンドで必要なドキュメントを作成します。後の演習で、その成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を準備し (前述のワークフローを実行)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. ToDo リストを確認し、Copilot が実行するタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、適切な計画を考えて決定し、コードを作成するまでの流れを確認できます。

この作業には数分かかります。Copilot cloud agent の主な目的の1つは、非同期にタスクを実行して、ほかの作業に集中できるようにすることです。この機能を活用するため、もう1つのタスクを Copilot cloud agent に割り当て、その間にアプリケーションへ機能を追加するコードを作成します。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の1つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は時間がかからないかもしれませんが、その時間をほかのタスクに使えます。この作業も Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使用します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue の作業を開始したことを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、作業中に監視し続ける必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期に作業を進められます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

この演習では、次の内容を学習しました。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- Issue の範囲を明確にすることの重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業する間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
