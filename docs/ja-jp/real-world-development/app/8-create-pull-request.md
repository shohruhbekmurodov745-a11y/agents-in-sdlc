---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの別々の PR に分けます。ワークショップを円滑に進めるため、フィルター機能と品質に関するワークフロー全体を1つのセッションとブランチで進め、そのすべての作業をこの PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学ぶ。
- 機能の PR 全体と検証の証拠を確認する。
- レビュー後にのみ Agent Merge を承認し、PR のマージを確認する。

## シナリオ

フィルター機能のワークフロー全体を通じて、Copilot を使用して機能の計画、実装、検証を行いました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、アプリのセッションがプルリクエストを読み取り、失敗した CI チェックの修正、レビューコメントへの対応、必要なリベースなど、マージを妨げている問題に対処し、GitHub で許可され次第マージします。バックグラウンドで動作し、アプリを再起動しても継続し、プルリクエストがマージされると自動的に無効になります。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、コードの編集とマージには明示的な承認が必要です。マージを許可する前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードを作成できたので、まとめてレビューし、Agent Merge に PR の残りのプロセスを管理させます。

1. エージェントピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンの項目を選択するだけでは開始されません。

Agent Merge が開始され、次の処理を行います。

- タイトルと説明を含むプルリクエストを作成します。
- セッションを issue から開始した場合は、説明の本文で関連する issue を参照します。
- リベースを行うか、ターゲットブランチとのマージ競合に対処します。
- CI を監視し、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックを確認し、コメントに対応して変更を更新します。
- 必要に応じて、すべての処理が成功した後に PR を自動的にマージします。

すべてのチェックが成功した後、Agent Merge に PR をマージさせます。

5. マージを許可する前に、PR の差分と検証の根拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** にチェックが付いていることを確認します。
8. 次のように明示して、エージェントにマージを依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 作業の進行状況を確認し、報告されたブロッカーに対処します。続行する前に、PR の状態が **Merged** であることを確認します。マージ権限の有効化やエージェントの実行中表示は、マージ完了の確認にはなりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前にそれらの阻害要因を解消してください。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次の作業を行いました。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学習した。
- 機能の PR 全体と検証の証拠を確認した。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認した。

次は、[既存のキャンバスを使い、トリアージキャンバスを作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app での Issue と pull request の管理][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app