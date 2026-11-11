---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使用するデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ独立したワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールして、そのリポジトリを接続します。

このレッスンでは、次の操作を行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで用意されたバックログなど、ワークスペースを確認します。
- クイックチャットを試して、アプリについて学びます。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを導入しています。Copilot app を使うと、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または Pro、Pro+、Business、Enterprise の有料プランを利用してください。Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストールが完了したら、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データレジデンシーを利用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選び、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択する画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリ選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選びます。前提条件のレッスンで控えた所有者名とリポジトリ名を指定します。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは `tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには次の領域があります。

- **New** - Copilot との新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合でプルリクエストを表示します。アプリを離れずに PR の参照と絞り込み、CI 状態の確認、レビューができます。
- **Issues** - issue を参照して絞り込み、issue からセッションを開始します。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。やることリストや定期的な保守などを任せるのに役立ちます。振り返りでは次のステップとして紹介します。このワークショップの演習には含まれません。
- **Customize** - MCP サーバー、プラグイン、スキルなどを追加して Copilot app の機能を拡張します。Playwright MCP を設定するときに使います。
- **Chats** — ブランチや専用ワークスペースを必要としない、質問やブレインストーミングのための軽い会話です。最初のチャットを始めると表示されます。表示されていない場合は **Sessions** の横の **+** を選択して最初のチャットを開始します。
- **Sessions** — リポジトリのフォルダーとそのエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリビューが開き、その下のセッションを選ぶと会話に戻ります。セッションはそれぞれ独立したワークスペースで実行されるため、変更を競合させずに複数を同時に実行できます。星評価を追加するときに最初のセッションを開始します。

ワークショップを進めながら、ワークスペースをさらに確認します。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能か知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリフォルダーを選びます。フォルダーの下にある個別のセッションではありません。リポジトリの上部ナビゲーションで **Issues** を選びます。
2. issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して詳細を読みます。各 issue からエージェントセッションを開始できます。簡単な最初の変更が終わったら、フィルタリングの issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。ほかのリポジトリの作業項目を確認するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリの使い方を学ぶには、アプリ自体について質問するのが効果的です。**クイックチャット**を使うと、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、その場限りの質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択して、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。チャットの後に **Chats** が表示されます。
2. 次の質問で、アプリのセッションについて尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して動かせることが分かります。会話は続けることも、新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次の操作を学びました。

- アプリをインストールして GitHub にサインインします。
- GitHub リポジトリからプロジェクトを追加します。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけます。
- クイックチャットで、その場限りの質問をします。

次は[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
