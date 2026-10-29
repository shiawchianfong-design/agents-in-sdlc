---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルター機能を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して調べる。
- QA エージェントを選択し、フィルター機能の Issue に照らして所見をレビューする。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタムエージェントを使って QA プロセスを調整し、再利用できるレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する、Copilot の特化した役割です。プロファイルにエージェントの目的、指示、利用可能なツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、Copilot がこれらの機能を使って要件を評価し、所見を報告する方法を説明します。これらの機能の代わりになるものでも、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成させます。プロファイルには、QA で実行するチェックと、従うべき制約の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトでカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使用する前に、そのプロファイルに意図した QA の手順と制約が含まれていることを確認します。検証だけを依頼したのに、エージェントが機能を変更してしまうのを防ぐためです。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に許可を求め、変更をコミットせず、プル リクエストを作成しないことを確認します。
5. これらの役割や制約が不足していたら、続行する前に既定のエージェントへ修正を依頼します。

## Issue を使って QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後、同じフィルター機能の会話を再開し、QA を選択します。これにより、すでに共有した Issue と計画の内容を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、エージェントのレポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、その成果をレビューしました。このレッスンでは、次の内容を学習しました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認した。
- 再利用可能な品質保証プロファイルを作成して調べた。
- QA エージェントを選択し、フィルター機能の Issue に照らして所見をレビューした。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、それらを[機能 PR にまとめ、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
