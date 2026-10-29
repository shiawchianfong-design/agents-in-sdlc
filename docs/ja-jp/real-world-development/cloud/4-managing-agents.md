---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザーエクスペリエンスの改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。たとえば、GitHub.com の[エージェントページ][agents-page]を使うと、オープンなプル リクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次の内容を学習します。

- エージェントページで cloud agent のタスクを監視する。
- 進行中のセッションに指示して、機能を追加で依頼する。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているため、ライトモードも含めるのが自然です。エージェントの作業に指示を出し、ハイコントラストモードに加えてライトモードも追加してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にタスクが表示されます。Copilot に割り当てたタスクのうち、次のようなものがあることを確認します。
    - コードのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを1つ選択します。Copilot が行った操作を確認します。次のような操作が含まれる場合があります。
    - リポジトリからコードをチェックアウトする。
    - Copilot が作業するための環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するための作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。ここから PR と変更されたファイルを確認し、さらに詳しく監視できます。

## cloud agent に指示を出す

実行中のタスクを確認できたので、cloud agent がハイコントラストモードを作成している間に、ライトモードも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。開始後は、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、Steer active session while copilot is working ダイアログが強調表示された cloud agent タスクの画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を任せる

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順を完了すると、新しい依頼が作業に追加されます。

今回も数分かかります。これまでに学習した内容を振り返りながら待ちましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して指示するための中心的な場所である、Copilot エージェントページについて学びました。この mission control を使うと、進行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに新しい指示を出せます。

この演習では、次の内容を学習しました。

- mission control とエージェントページを使って cloud agent のタスクを監視する。
- 進行中のセッションに指示して、機能の追加を依頼する。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した[プル リクエストを改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
