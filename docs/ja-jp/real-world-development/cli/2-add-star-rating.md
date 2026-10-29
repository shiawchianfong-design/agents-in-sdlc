---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小規模なものにします。ゲームのデータにはすでに星評価が含まれていますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次の内容を学習します。

- 機能ブランチで対象を絞った Copilot の会話を開始する。
- プロジェクトに小さな変更を加えるようエージェントに依頼する。
- `/diff` で変更をレビューする。
- アプリを実行し、転送されたブラウザーで変更を確認する。
- 最初のプル リクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。最初の作業として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションに最適な変更です。

## 会話の構造

**会話**では、Copilot CLI とタスクに取り組みます。Copilot app と異なり、通常の CLI 会話は専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存した会話に戻ることができますが、ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの回答、ファイルを調査および編集するエージェントのツールアクティビティ、`/diff` で確認できる変更という3つの要素を扱います。

## 会話を開始して変更を依頼する

新しい会話を開始して、機能を実装します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけて、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

AI が生成したすべての変更は、どれほど小さくてもマージ前にレビューする必要があります。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. ゲームカードに、評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に決定論的ではなく確率的に動作するため、実際のコードは異なる場合があります。特定の実装に固執せず、動作を確認してください。

## 変更を確認する

コードを読むだけで動くと判断するのではなく、Copilot に Web サイトを起動させ、Codespaces で転送されたブラウザーを使って UI を確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら **Open in Browser** を選択します。
3. ゲームカードに5点満点の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトを使って、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。次は、新しいコードをプロジェクトにマージするためのプル リクエスト (PR) を作成します。

1. 既定のエージェントに次のプロンプトを送り、変更をコミットします。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次の作業を行いました。

- 機能ブランチで対象を絞った Copilot の会話を開始した。
- ゲームカードに小さな変更を加えるようエージェントに指示した。
- `/diff` で変更をレビューした。
- アプリを実行し、転送されたブラウザーで星評価を確認した。
- 最初のプル リクエストを作成してマージした。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使用して][next-lesson]、より大規模な機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
