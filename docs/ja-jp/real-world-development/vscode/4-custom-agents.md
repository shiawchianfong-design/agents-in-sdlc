---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義すると、特定の作業を効果的に実行するための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング方法を確認するセキュリティエージェント、一貫したテストパターンを適用するテストエージェントなどがあります。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重複しています。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを調整するように設計されています。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プル リクエストのワークフローを支援するスキルや、テストを実行、管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI を使うほかの作業と同様、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザインチームから、ユーザーが切り替えられるハイコントラストモードの実装が依頼されました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントに機能を生成させます。

この演習では、次の内容を学習します。

- 既存のアクセシビリティカスタムエージェントを確認する。
- Copilot Chat でアクセシビリティエージェントを使用し、ハイコントラストモードを実装する。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。内容を確認し、Copilot への指示を把握しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します (演習 3 のフィルター PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき点。

## Copilot Chat でカスタムエージェントを使用する

VS Code では、`.github/agents` に定義されたすべてのカスタムエージェントが、Copilot Chat ビュー下部のエージェントドロップダウンに表示されます。カスタムエージェントを選ぶと、そのエージェントの指示とツールに対象を絞ったチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. Chat ビューのエージェントドロップダウンで、まだ選択されていなければ **Agent** を選択します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを選択します。ここには現在のエージェントが表示されます。既定では **default** です。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに、次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始し、コードベースを調べて変更を提案し、プロジェクトファイルを編集します。編集内容は、ファイルパスと差分とともにチャットに表示されます。
6. 編集が追加されると **Files changed** の表示が更新され、エージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。作業中に Astro コンポーネント、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、この進行中の作業を確認し、エージェントに追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できる形にまとめ、Copilot が特定の作業を効果的に進めるための指針にできます。

この演習では、次の内容を学習しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使用する方法。

次は、エージェントの作業を[監視して方向を調整し][next-lesson]、変更を確認しながら、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
