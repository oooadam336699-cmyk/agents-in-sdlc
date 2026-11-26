---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとのやり取りをすべてコマンド ラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップは、Copilot CLI をインストールすることです。使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI は、開発者が日常的に使うターミナルからその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys のリポジトリであることを確認します。**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま続けてください。

## 使い方を確認する

通常の shell prompt で入力するコマンドは、Codespace で直接実行されます。Copilot CLI が起動すると、自然言語はエージェントに送信され、slash command は会話を操作します。

1. `/model` を入力します。矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプを閉じます。
3. 次のような簡単な質問を Copilot に送り、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のように依頼して、フィルター機能の Issue を見つけます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue を読みます。最初の変更を行った後、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションでは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree が作成されるわけではありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、許可された範囲外のツールを使うときに、通常は確認を求めます。このワークショップでは `--yolo` を付けて再起動します。ユーザーが許可したこのショートカットを Codespace 内で使うと、承認のプロンプトが省略され、演習に集中できます。

> [!CAUTION]
> `--yolo` は自動的にすべてを許可する設定（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を指定して、後のレッスンで Issue やプルリクエストに使う GitHub MCP の読み取り / 書き込みツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう1つ簡単な質問をして会話が動作することを確認し、`/exit` を入力して終了します。

Copilot は会話を自動的に保存します。後のレッスンでは指示を変更したりエージェントを追加したりした後に `copilot --resume` を使い、同じ機能の会話とブランチを再開します。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使ってフィルター機能の Issue を見つけました。

次は[最初の変更に取り組み][next-lesson]、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
