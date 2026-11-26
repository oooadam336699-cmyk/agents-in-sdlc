---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が参照できるようにしておきます。[Instruction file][instruction-files] では、作成するコードの内容だけでなく、構成方法も説明できます。フィルター機能を作成したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の instruction file がエージェントにどのように渡るかを確認します。
- コーディング標準が守られるよう、指示ファイルを更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントでは、コードの内容を言い換えるのではなく、その意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export された関数では、`db` 引数を注入できる場合はその点も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたらコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

Instruction file を使うと、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡せます。

## Instruction file

カスタム指示では、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解させます。これは提案やコード スニペットをより適切なものにするための強力な機能です。使用するコーディング規約やライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスク単位の指示も作成できます。

Instruction file は次の2種類です。

- `.github/copilot-instructions.md` は、このリポジトリへの**すべての**リクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイダンスを記載します。

> [!NOTE]
> ほかの instruction 形式やそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更の影響を確認する前に、スターター プロジェクトに用意されている instruction file を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングに関するガイダンスを確認します。これらの指示は、このリポジトリで Copilot とやり取りするたびに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内のファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot が参照する例を示せます。

## チームのガイダンスに合わせて指示を更新する

用意されているファイルはよい出発点ですが、新しく生成される TypeScript に TSDoc コメントを追加するため、中心となる `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま更新後の指示を読み込めるよう、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパス単位の instruction file がエージェントにどのように渡るかを確認しました。
- コーディング標準が守られるよう、指示ファイルを更新しました。
- instruction file がコードに与える影響を確認しました。

次は[quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
