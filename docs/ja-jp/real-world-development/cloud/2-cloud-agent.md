---
title: "レッスン 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能要求などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確に定めることの重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解消したい技術的負債があります。最初のサイトを構築した委託先が、十分なドキュメントを残していません。まず、アプリケーションのすべてのエクスポート関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示します。完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。このタスクは現在ブロックされていますが、ほかに優先度の高い Issue があります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、非同期で作業を進めてもらい、開発者は別の作業に集中できます。完了後に戻って、期待どおりの結果かをレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。セッションの進行中でも完了後でも、その内容を確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な作業でも、詳しく見ると複雑なことがあります。[Copilot cloud agent にタスクを依頼するときは、範囲を明確に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前のレッスンでは、カスタム指示とその役割を確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認して、後で指示の影響を見つけられるようにします。

リポジトリの github.com の Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規則と TSDoc doc comment の要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、プルリクエストに TSDoc doc comment、TypeScript の規則、コメント ヘッダーが反映されているかを確認します。後のレッスンで PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードの作成には特定の開発環境と、必要なツールを準備するセットアップ スクリプトが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に環境をカスタマイズできます。必要な開発ツールや依存関係を使えるようにします。このワークショップでは学習を進めやすくするため、あらかじめ設定しています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して最低限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなどでセットアップ手順内からリポジトリをクローンする場合は、`contents: read` 権限が必要です。クローンしない場合は、セットアップ完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot がプルリクエストの作業を始める前に実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動で実行できます。Copilot が実行するまで待たずに、ワークフローをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、最新情報が記載されていないプロジェクトや、ドキュメントがまったくないプロジェクトもあります。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成を得意としているため、cloud agent に Issue を割り当てるのに適しています。バックグラウンドで必要なドキュメントを作成してもらい、後のレッスンでレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot が To Do リストを作成するまで数分かかることがあります。Copilot は環境を作成して（前述のワークフローを実行し）、プロジェクトを分析したうえで、問題に取り組む方法を判断しています。

13. リストと、これから実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウの再読み込みが必要なことがあります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、方針を考え、コードを作成していく過程を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期で進めて開発者が別のタスクに集中できるようにすることです。この機能を活用して、もう一つのタスクを Copilot cloud agent に割り当て、その間にアプリケーションへ機能を追加しましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チーム向けの関連ゲームのセクションがすぐに終わるとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目のタスクにも取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、完了まで積極的に監視する必要はありません。完了後にレビューしましょう。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。Issue を Copilot に割り当てると、バックグラウンドで非同期に作業します。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などを任せられます。

次の概念を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確に定めることの重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。これらは前のモジュールで確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
