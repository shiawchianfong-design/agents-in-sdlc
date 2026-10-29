---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を繰り返し計画してコードを作成し、動作をテストし、プル リクエストをレビューし、エージェントと作業を管理します。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディングサービスに取り組みます。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。フィルターやページネーションなどの機能に加え、アクセシビリティやコーディング標準などの品質改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップは独自のセットアップ手順から始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使用します。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、エージェントが作成したプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
