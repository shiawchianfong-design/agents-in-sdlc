---
title: "レッスン 6 - Playwright MCP による機能の検証"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が利用できる自動化機能の追加などが必要です。MCP server を使うと、Copilot CLI に組み込まれている機能以外のツールやサービスも利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを理解する。
- Playwright MCP server がまだ利用できない場合は追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

単体テストや E2E テストも重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように Web サイトを使って機能を確認できれば、変更が期待どおりに動作する確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスの間を仲介する MCP server を通じてアクセスします。各 MCP server は、エージェントが利用できるツールやリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理するための API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

MCP server はほかにも数多くあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP server はプロジェクト内の他の依存関係と同じように扱ってください。使用する前に、ソースコードを確認し、発行元を検証して、セキュリティ上の影響を検討します。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるよう追加します。

> [!NOTE]
> Playwright MCP が Copilot CLI にすでに設定されている場合は、セットアップ手順をスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server の種類として **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を確認するよう Copilot に依頼する

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP server を使って、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP server を見つけるため、明示的に指定しなくてもかまいません。ただし、使用するツールがわかっている場合は、指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を確認します。
3. 結果の報告を読み、すべて期待どおりに動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作してから server を停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使い、実際のブラウザーで機能を確認しました。次の内容を学習しました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを理解した。
- Playwright MCP server がまだ利用できない場合は追加した。
- エージェントにブラウザーを操作させ、フィルター機能を確認した。

次は、スキルとブラウザーツールを専門的な役割としてまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
