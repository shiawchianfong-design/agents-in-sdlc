---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが用意されています。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する仕組みを確認する。
- `/model` を使い、利用可能なモデルを確認する。
- `/share` を使ってセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認する。

## シナリオ

CLI の中心的なワークフローは完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

規模が大きく複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、コンテキストを自分で確認したり圧縮したりすることもできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを自分で決めたいときは `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を開始する場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者の好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学びを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、目的に合った方法を選択します。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 任意: 作業の並列化または委任

ワークショップの中心的な内容は完了しました。Copilot CLI には、大規模なタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割して並列に実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期に動作してプル リクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試すときは、新しく範囲を明確にしたタスクから開始し、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業をさらに詳しく確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の状況を確認したりできます。このレッスンでは、次の内容を学習しました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する仕組みを確認した。
- `/model` を使い、利用可能なモデルを確認した。
- `/share` を使ってセッションをエクスポートまたは共有する方法を学んだ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は、[学んだ内容と次のステップ][next-lesson]を振り返りましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
