---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、プル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを円滑に進めるため、フィルター機能と品質に関するワークフロー全体を1つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを理解する。
- 機能の変更全体と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR のマージを確認する。

## シナリオ

フィルター機能のワークフロー全体を通じて、Copilot を使用して機能の計画、実装、検証を行いました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プル リクエストをマージするまでの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビューコメントへの対応、必要に応じたリベースなど、PR を妨げている問題を解決します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge に任せることもできます。Agent Merge を有効にする前に、作業をレビューし、マージできる状態であると判断してください。

## Agent Merge で PR を管理する

コードの作成が完了したので、まとめてレビューし、PR を作成して、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを入力して新しい PR を作成します。
5. `/pr agentmerge` を入力して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視し、すべて成功したらマージします。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次の作業を行いました。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学習した。
- 機能の変更全体と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に役立つ[追加の Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
