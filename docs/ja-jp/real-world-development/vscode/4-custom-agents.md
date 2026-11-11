---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせて、特化型の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、作業を効果的に進めるための指示、ベストプラクティス、コーディングパターン、領域固有の知識を Copilot に伝えます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなど、チームの知識を再利用できる形にできます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、または `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述した Markdown プロンプトを含めます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言えば、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを組み合わせて作業を進めます。このワークショップでは、アクセシビリティのカスタムエージェントがサイトを確認してガイドラインに沿って更新します。その過程で、プルリクエストのワークフローやテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、使う環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディングのプラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、文字色と背景色のコントラストが十分でないため、現在のダークテーマを読みにくいと感じるユーザーがいます。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。カスタムエージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat のアクセシビリティエージェントで、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのように導くかを確認します。

codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します（演習 3 のフィルタリング PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成で担う主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべきこと。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したカスタムエージェントが Copilot Chat ビュー下部のエージェント一覧に表示されます。カスタムエージェントを選ぶと、そのエージェントの指示とツールに沿ったチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. まだ選択されていない場合は、Chat ビューのエージェント一覧から **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント一覧を選択します。ここには現在のエージェントが表示され、既定では **default** です。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクトのファイルに適用します。各編集はファイルパスと差分とともにチャットに表示されるため、確認できます。
6. エージェントが編集したファイルの一覧は、**Files changed** の表示に反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリを実際に変更します。作業中は Astro コンポーネント、CSS、関連するテストなどを編集します。

次の演習では、実行中の作業を確認して方向づけます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや領域に特化した AI アシスタントを使うと、チームの知識や標準を再利用できる形にして Copilot の作業に反映できます。

このレッスンでは、次の内容を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は、エージェントの変更を確認し、同じセッションにライトモードの切り替え機能を追加するため、[作業を監視して方向づけます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
