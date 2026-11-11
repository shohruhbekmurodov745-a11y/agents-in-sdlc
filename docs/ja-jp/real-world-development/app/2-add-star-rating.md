---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

前のレッスンでは、ワークスペースを確認し、クイックチャットを試しました。次は**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェントセッションを開始し、その構成を学びます。
- プロジェクトに小さく範囲を絞った変更を加えるよう依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリー、発行元、説明しか表示されていません。練習として、各カードに既存の評価を表示するようエージェントに依頼します。小さく自己完結した変更なので、最初のセッションに最適です。

## セッションの構造

**セッション**は、独立した専用ワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方のセッションで機能を追加しながら、別のセッションでバグを修正するなど、変更を競合させずに複数の作業を同時に進められます。セッションはサイドバーにリポジトリごとに表示され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調べて編集するエージェントの**ツール操作**、差分付きの**変更ファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを始めて、プロジェクトを確認し、機能を実装します。[アプリのセットアップ][prior-lesson]で GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻ります。アプリを閉じている場合は起動します。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを送信します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである worktree を作成します。続いてプロジェクトを調べ、新機能に必要なファイルを見つけてコードを作成します。Copilot app で新しい機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot app 内で変更を確認します。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームカードを表示する主要ファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` の場合は "No rating yet" に置き換える、小さなコードブロックが追加されているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> ほかの生成 AI ツールと同様に、Copilot は確率的に動作します。実際のコードは上記と異なる場合がありますが、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と `null` の代替表示の両方をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされたりしている場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動してもらい、更新された UI を確認します。サイトを起動してブラウザーキャンバスで開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot app 内に表示されるインタラクティブなウィジェットです。後のレッスンでカスタムキャンバスを試して作成しますが、ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトで、アプリを起動してブラウザーキャンバスに表示するよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーが開きます。
3. 評価済みのゲームカードに 5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションで起動した開発サーバーを停止するよう依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能が完成したので、新しいコードを既存のコードベースにマージするためのプルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. PR を作成する前に該当する issue を用意する必要があると Copilot から通知された場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。必要に応じてレビューします。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使い、最初の変更をリリースできました。具体的には、次の操作を行いました。

- エージェントセッションを開始し、その構成を学びました。
- ゲームカードに小さく範囲を絞った変更を加えるよう依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は、[フィルタリングの issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot app でエージェントセッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
