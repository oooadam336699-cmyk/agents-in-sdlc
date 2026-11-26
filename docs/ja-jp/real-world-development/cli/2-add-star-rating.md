---
title: "レッスン 2 - 星評価を追加する: 小さな変更から始める"
description: "Copilot CLI でゲームカードに小さな変更を加え、ポートフォワードされたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えましょう。変更は小さくします。ゲームのデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示するよう依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を始めます。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- ポートフォワードされたブラウザーでアプリを起動し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホームページのゲームカードに表示されるのは、タイトル、カテゴリ、出版社、説明だけです。ウォームアップとして、エージェントに既存の評価を各カードへ表示させます。小さく独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**では、Copilot CLI と一緒にタスクに取り組みます。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作成せず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話に戻って同じ話題を続けられますが、ファイルとブランチはディスク上の通常の Git 状態として管理されます。

会話には、あなたのプロンプトとエージェントの応答、ファイルを調査・編集する際のツール操作、`/diff` で確認できる変更の3つが表示されます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
3. まだ起動していない場合は、次のコマンドを実行して Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを特定して、必要なコードを作成します。これで Copilot CLI を使った新機能の追加は完了です。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲームカードに表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot は決定論的ではなく確率的に動作するため、生成されるコードは環境によって異なる場合があります。特定の実装と完全に一致することではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces でポートフォワードされたブラウザーから更新された UI を確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を実装できました。変更をプロジェクトにマージするため、プルリクエスト（PR）を作成します。

1. 既定のエージェントに変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. <kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで目的を絞った Copilot の会話を始めました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- ポートフォワードされたブラウザーでアプリを起動し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[フィルタリングの Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を実装します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
