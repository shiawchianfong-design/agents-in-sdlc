---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプル リクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

一連の Tailspin Toys ワークフローを通じて、GitHub Copilot CLI を使用しました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと用意されたフィルター機能の Issue を確認した。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージした。
- フィルター機能の Issue から開始し、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューした。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルター機能を確認した。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価した。
- フィルター機能の変更全体をレビューし、フィルター PR に Agent Merge を許可した。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認した。

## リリースしたもの

ワークショップには、更新済みの `main` からそれぞれ別のブランチで作成する、2つの PR があります。

1. **星評価:** ゲームカードに既存の `starRating` と未評価であることを示す表示を追加する。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` の報告形式を変更し、QA プロファイルを作成して関連テストを含める。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを簡略化するため、これらの作業を1つの PR にまとめました。

## 異なる種類の検証

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR の前にまとめて確認しました。

追加したテストで実際の不足を解消できることが重要です。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠をレビューしてください。変更があった場合は、影響する証拠を再確認します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP server を設定しました。これらのカスタマイズは会話をまたいで再利用でき、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけで判断せず、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前にアプローチを検討するときは **Plan**、対象を絞った変更に関与し続けたいときは **Interactive**、範囲が明確なタスクには **Autopilot** を使用します。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択できます。

コンテキストは、基盤となる仕組みと同じくらい重要です。*何を*作りたいか、*なぜ*必要か、*どのように*進めたいかを明確に説明すると、出力は大きく変わります。

## さらに確認する

中心的なワークフローを完了しました。Copilot CLI には、ほかにも便利な機能があります。

- `/review` を使って、コードレビューエージェントに変更を分析させる。
- `/rubber-duck` を使って、問題について対話し、別の見方を得る。
- `/fleet` を使って、独立したサブタスクを並行して調整する。
- `/worktree` を使って、別のタスクを分離する。
- `/delegate` を使って、Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、何年も考えていたけれどまだ作っていない小さなアプリなど、さまざまな場面で活用してください。学びをチームと共有し、チームからも学んでください。そして、ドキュメントも確認しましょう。

ターミナルから Tailspin Toys をさらに拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
