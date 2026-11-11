---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業内容をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードのドキュメント追加、デザインチーム向けの関連ゲーム機能、高コントラストとライトモードの切り替えなどのアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善します。

### シナリオ

何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監視されていない状態で非同期にタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、コードを変更するときは自身が使用するブランチにのみ書き込みます。
- cloud agent は GitHub Actions 内で、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。Copilot の作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードを確認します。TSDoc doc comment やその他のドキュメントが追加されています。変更内容は状況によって異なります。

    変更を確認するときは、演習 2 で確認した指示ファイルによる TSDoc doc comment、TypeScript の規約、コメントヘッダーに注目してください。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。すべて問題なければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。ほかのチームメンバーと同様に、プルリクエストのコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、追加のセッションを開始して対応します。結果は確率的に変わるため、固定のプロンプトは示せません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc doc comment を追加します。
- Astro アプリの構成を説明する README を作成します。

1. 上記の例などを使い、コードベースに追加したいドキュメントの変更を **@copilot** にメンションしてコメントで依頼します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

問題がなくなるまで繰り返し改善できます。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![レビュー可能な状態に PR を変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲームの表示について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。すべて問題なければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Copilot が作成したブランチを Codespace で開き、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を実装しました。Copilot cloud agent には引き続き変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、カスタム指示を追加する前に割り当てたタスクにコメントヘッダーや TSDoc doc comment を追加したりできます。前と同じように **Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で依頼したライトモードが含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定の保存に使うローカルストレージ。
   - ハイコントラストモードとライトモードの CSS またはスタイル変更。
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。すべて問題なければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace で Copilot が作成したブランチを開き、アクセシビリティ機能を手動でテストできます。ターミナルを開いて、Copilot が作成したブランチ名に `<branch-name>` を置き換え、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が保存されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。アクセシビリティの問題や改善点があれば、前の PR と同様にコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 委任を続ける

実際のバックログ項目をエージェントに任せ、自分は別の作業に移ると、cloud agent を効果的に活用できます。習慣にするため、リポジトリにいくつか issue を作成し、Copilot に割り当てます。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの興味登録フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent のハーネスを完了しました。各レッスンで、次の操作を行いました。

- このリポジトリにあるカスタム指示ファイルを確認し、後で cloud agent の出力に反映されることを学びました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストモードとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティのセッションを監視して調整しました。
- Copilot のプルリクエストをレビューして改善し、**@copilot** に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を広げられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への引き継ぎを使い、ターミナルからレビュー済みの変更を 2 つ作成します。

自分のリポジトリでは、次のアイデアも試してみてください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野を担当するカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent ファイアウォールを設定する][agent-firewall]

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
