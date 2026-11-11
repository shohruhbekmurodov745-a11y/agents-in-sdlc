---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

この数回の演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに追加の指示を与えることもできます。

このレッスンでは、次の操作を行います。

- cloud agent のタスクを監視するために、エージェントページを確認します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードを作成するようエージェントに割り当てた後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新する作業をすでに進めているため、ライトモードも含めるのが合理的です。エージェントの作業に指示を追加し、ハイコントラストに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てた次のタスクなどが確認できます。
   - コードベースのドキュメントの更新。
   - ゲーム変更用 API の生成。
   - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次の内容が含まれる場合があります。
   - リポジトリのチェックアウト。
   - Copilot の作業環境の作成。
   - MCP サーバーの設定。
   - 割り当てられたタスクを完了するための各種手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や取り組み方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更ファイルを確認し、さらに詳しく監視できます。

## cloud agent に追加の指示を与える

実行中のタスクを確認したので、エージェントがハイコントラストモードに取り組んでいる間に、ライトモードのトグルも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、セッションを数分間確認します。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、Copilot の実行中のタスクに追加指示を与えるダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を確認し、作業計画に追加したことを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼が作業に反映されます。

作業には数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して案内する中心的な場所である、Copilot agents ページについて学びました。mission control を使うと、進行中と完了済みのタスクを追跡し、実行中の作業を確認して、進行中のタスクに追加の指示を与えられます。

このレッスンでは、次の操作を行いました。

- mission control とエージェントページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
