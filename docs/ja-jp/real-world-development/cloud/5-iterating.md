---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善と機能追加を目的とする複数のタスクで、GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチームが改善を検討できる関連ゲーム機能の作成、ハイコントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、改善してもらいましょう。

### シナリオ

これまでに何度も確認したように、生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は、非同期かつ監督者なしでタスクを実行します。安全性を確保するため、次のようなセキュリティ上の制約があります。

- Copilot はリポジトリに対する読み取りアクセス権を持ち、書き込みアクセス権はコードに使用するブランチに**のみ**付与されます。
- cloud agent は GitHub Actions 内で動作し、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR) を確認します。コードへのドキュメント追加を扱います。GitHub.com の標準的な PR インターフェイスを使って確認します。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プル リクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプル リクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学習した内容を振り返ったりしてください。

4. プル リクエストの準備ができたら、**Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメントコメントや、そのほかのドキュメントを含むコードを確認します。変更内容は状況によって異なります。

    変更を確認しながら、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルから反映されています。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プル リクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プル リクエストで Copilot と作業するのは、一方通行ではありません。チームメンバーと同じように、プル リクエストのコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼する文章を1つに決めることはできません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、ファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例から選ぶか、コードベースに追加したいドキュメントについて別の提案を使って、**@copilot** をメンションするコメントを追加します。ほかのユーザーをメンションする場合と同じ方法です。
2. **View Session** を選択して、Copilot が作業する様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択すると、プル リクエストに戻れます。

    ![プル リクエストに戻る画面](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プル リクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足できるまで、Copilot に変更を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. プル リクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Copilot が作成したブランチに切り替えて、Codespace で関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の作業と同様に、Copilot cloud agent に更新を依頼して、繰り返し改善できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーと TSDoc のドキュメントコメントが追加されていることを確認したりできます。なお、このタスクはカスタム指示を更新する**前**に割り当てたものです。前と同じように、**Conversation** タブにコメントを追加すると、Copilot がそれを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するための local storage の使用。
   - ハイコントラストモードとライトモードに関する CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プル リクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Copilot が作成したブランチに切り替えて、Codespace でアクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を導いたことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せれば、その間に別の作業を進められます。この方法を身につけるために、リポジトリにいくつか Issue を登録し、Copilot に割り当ててみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンでは、次の内容を学習しました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力にどのように影響するかを確かめました。
- **Issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の作業を確認しました。
- **カスタムエージェントを作成して使用し**、ハイコントラストモードとライトモードを追加しました。
- **Copilot エージェントページを mission control として使用し**、アクセシビリティセッションを監視して作業の方向を調整しました。
- **cloud agent のプル リクエストをレビューして改善し**、`@copilot` に変更を依頼したり、ワークフローを承認したりしました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学習するには、ここで取り組んだ内容を補完するほかのハーネスもご覧ください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から離れずに Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使い、レビュー済みの変更を2つターミナルから作成します。

自分のリポジトリで、次のアイデアを試してみましょう。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
