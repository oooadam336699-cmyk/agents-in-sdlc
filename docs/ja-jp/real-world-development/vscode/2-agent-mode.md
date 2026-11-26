---
title: "レッスン 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行してデバッグします。変更し忘れたファイルに気づけば、さらに更新することになります。作業は続きます。

このような場面で Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業するよう設計されています。既存のプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動で修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。Agent mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。Cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

このレッスンでは、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関係するファイルを特定し、複数の変更を調整する方法。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや publisher で絞り込めるようにします。前のレッスンで publisher ヘルパーを追加しました。次は Copilot Agent Mode を使い、残りのデータ層、UI、テストの作業を仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、ボード ゲーム向けのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリはビルド時にページを静的 HTML として生成します。ページはフロントマターから Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前のレッスンを終えていれば、`main` ブランチにいるはずです。
2. Codespace のターミナルを開きます。<kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押します。
3. フィルタリング機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ:** タイトル、画像、説明を含むボード ゲームの一覧が表示されます。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの情報を表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。該当する手順で説明します。

Web サイトは動作していますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトを作成すると、Issue のバックログが自動的に用意されます。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューから **Agent** を選択します。**Agent** は複数ファイルにまたがる変更を計画して実装し、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めて、受講者が進めやすくするためです。ただし、LLM の出力は確率的であるため、手順と異なる提案が出る場合があります。これは正常な動作です。

> [!NOTE]
> Copilot は確率的に動作するため、別の MCP コマンドを使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先していることを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを提供する指示ファイルを確認しましょう。Web サイトへの機能追加の進め方を説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - 次のファイル種別に関する指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、指示を再利用し、大きな指示を小さなファイルに分け、参考例やひな形を示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

また、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらのタスクを依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを調べた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の手順を進めます。タスクやコードベースの複雑さによって数分かかることがあります。作業中、エディターに **Keep** と **Undo** のボタンが表示される場合があります。完了するとすべての変更に対して **Keep** または **Undo** を選択できるため、作業中に選ぶ必要はありません。

7. Copilot に確認されたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業の途中で一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで修正します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関係するファイルを特定し、複数の変更を調整する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に追加の変更を依頼し、コンテキストを伝えて反復する場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
