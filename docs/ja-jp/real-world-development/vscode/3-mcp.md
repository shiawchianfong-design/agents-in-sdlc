---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使って、フィルター機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する仕組みを理解する。
- Playwright MCP server をワークスペースに追加する。
- Copilot に Playwright MCP server を使わせて、ブラウザーでフィルター機能を手動テストする。
- ブランチを公開し、フィルター機能のプル リクエストを作成する。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールを利用できると、さらに便利になります。Copilot は Model Context Protocol (MCP) を使ってツールにアクセスします。MCP は、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードがコンテキスト、LLM、MCP server と VS Code 拡張機能が提供するツールなどと連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部ツールやサービスとリアルタイムにやり取りできます。これにより、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスの間を仲介する MCP server を通じてアクセスします。MCP server は、既存の API や npm パッケージなどのローカルツールを含む外部ツールと AI エージェントの通信を管理します。各 MCP server は、AI エージェントが利用できる別々のツールやリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するための一連の API を提供します。AI エージェントは、新しいリポジトリの作成、既存のリポジトリの更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースを提供する MCP server は、ほかにも数多くあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP server はプロジェクト内の他の依存関係と同じように扱ってください。使用する前に、ソースコードを慎重に確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できる MCP server のみを使用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP server を設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` server がすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP server][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が操作できるブラウザーを利用でき、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` server と異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright に画面のないブラウザーを実行するよう指定します。デスクトップ画面がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは E2E テストに Playwright を使用しているため、通常は Playwright 用のブラウザーもインストール済みです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して、もう一度試します。

## server を起動して信頼する

VS Code は MCP server を**必要に応じて起動**します。Copilot が server のツールを初めて必要としたときに起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択して server を起動し、構成が正しいことを確認します。
2. VS Code で server を信頼するか確認されたら、構成をレビューし、信頼することを選択して起動します。

手動で起動する必要はありません。スキップした場合は、次の手順で Copilot が初めてツールを必要としたときに VS Code が server を起動し、信頼するかどうかを確認します。

## フィルター機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。ユーザーがアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、確認した内容をまとめます。会話を離れずに機能が期待どおりに動作するか確認できます。

Playwright MCP server は、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。エージェントは、支援技術と同じようにボタン、リンク、リスト項目などの構造化されたラベル付き要素を処理します。そのため、基本的な機能確認をアクセシビリティの簡易チェックとしても活用できます。

server を接続し、アプリを実行した状態で、Copilot に作成したフィルター機能をテストするよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server を通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らし合わせて報告内容を確認してください。問題がある場合は、追加の質問をするか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストを実行するには、アプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP server を初めて使うとき、ブラウザーをダウンロードする必要がある場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようにプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使用しているので、PR の作成も依頼しましょう。関連する Issue を探し、Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使用できるように、エージェントドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを使って PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の収集や操作の実行に必要な作業を Copilot が行うため、求められたら **Continue** を選択します。GitHub ツールを初めて使用するときに GitHub への認証を求められることがあります。画面の指示に従って認証します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプル リクエストを確認します。ただし、まだマージ**しないでください**。

## まとめと次のステップ

この演習では、次の内容を学習しました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する仕組みを理解した。
- Playwright MCP server をワークスペースに追加した。
- Playwright MCP server を使って機能を公開する前に、ブラウザーで手動テストした。
- ブランチを公開し、フィルター機能のプル リクエストを作成した。

MCP server を使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の1つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
