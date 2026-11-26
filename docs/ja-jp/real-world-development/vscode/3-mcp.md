---
title: "レッスン 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent mode でフィルタリング機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作してもらいます。その後、ブランチを公開して PR を作成します。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Copilot がエディターの外部に接続できると、agent mode の機能が大きく広がります。Model Context Protocol (MCP) はそのための標準です。エージェントが外部のツールやサービスと通信できるようにします。

![agent mode の内部構成と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能から提供されるツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントは最新の情報（リソース）にアクセスしたり、ユーザーに代わって操作（ツール）を実行したりできます。

ツールとリソースへのアクセスは MCP サーバーを介して行います。MCP サーバーは AI エージェントと外部のツールやサービスとの橋渡しをします。既存の API や npm パッケージのようなローカル ツールとの通信を管理します。各 MCP サーバーが、AI エージェントから利用できるツールとリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** GitHub リポジトリを管理する API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントが実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけて貢献できる [MCP registry](https://github.com/mcp) を提供しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。利用する前にソース コードを確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれているため、前のレッスンで Copilot が Issue のバックログを確認できました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作可能なブラウザーを利用できます。今回の機能テストに適しています。

1. `.vscode/mcp.json` に `github` と並べて `playwright` の設定を追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright を画面のないモードで実行します。デスクトップ画面がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright をすでに使っているため、通常は Playwright が必要とするブラウザーもインストール済みです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたときに、VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認し、信頼して起動できるようにします。

手動での起動は任意です。省略した場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って報告します。会話を離れずに機能の動作を確認できます。

内部では、Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などのラベル付き要素を構造化された形で扱います。そのため、機能の確認と同時に簡単なアクセシビリティ チェックもできます。

サーバーを接続し、アプリケーションを起動した状態で、Copilot に次の指示を送ってフィルタリング機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を進めて結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。動作に問題がある場合は、追加の質問をするか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリケーションが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト（PR）を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。関連する Issue を探し、PR と関連付けてもらいます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. エージェント選択メニューで **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使うために必要です。
3. 次のように Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作に必要な権限を許可します。GitHub ツールを初めて使うときに GitHub 認証を求められた場合は、指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択してプルリクエストを確認します。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- MCP サーバーを使って、出荷前にブラウザー上で機能を手動テストしました。
- ブランチを公開し、フィルタリング機能のプルリクエストを作成しました。

MCP サーバーで機能をテストしましたが、MCP は Copilot を拡張する方法の一つにすぎません。次は、アクセシビリティ レビューなどの作業に役立つ[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
