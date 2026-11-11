---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合、そのガイドラインを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、作成するコードだけでなく、その構成方法も記述できます。フィルタリング機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる方法を確認します。
- コーディング標準に沿うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントには、コードの説明を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたときにコメントも更新します。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を理解するためのコンテキストや設定を Copilot に伝えられます。コーディング規約やライブラリ、追加したいコメントの種類を指定できます。リポジトリ全体に適用する指示も、特定のファイルを対象にする指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの指示ファイル形式やその対応状況は、環境によって異なります。特定の形式を利用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、作業を始めやすいよう指示ファイルが含まれています。変更を加える前に、既存のファイルを確認して効果を比べられるようにします。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、リポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内のファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

既存のファイルは有用ですが、まだ不足があります。コアの `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を更新したら、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま新しい指示を読み込ませます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新たに生成された TSDoc コメントがコードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる方法を確認しました。
- コーディング標準に沿うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な quality-checks スキルをカスタマイズして実行し、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
