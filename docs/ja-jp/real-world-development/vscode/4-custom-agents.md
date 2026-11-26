---
title: "レッスン 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、重点的な指示、ベスト プラクティス、コーディング パターン、分野固有の知識を定義すると、特定の作業をより効果的に進めるよう Copilot を導けます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーに置く Markdown ファイル、またはグローバルな `~/.copilot/agents` に置くファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタム エージェントと agent skills の違い

カスタム エージェントと [agent skills][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。最も簡単な区別は、**カスタム エージェントは作業者、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを使って作業を調整します。このラボでは、アクセシビリティのカスタム エージェントがサイトをアクセシビリティ ガイドラインに照らして確認し、更新します。その作業の中で、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルなどを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントの作成方法は一つではありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントを使って機能をすばやく実装します。

このレッスンでは、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、ターミナルを開きます。Exercise 3 のフィルタリング PR と分けるため、アクセシビリティ作業用に `main` から新しいブランチを作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` のフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティ対応の Web サイト用コードを生成する際の主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビューの下部にあるエージェント選択メニューには、`.github/agents` に定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。該当する手順で説明します。

1. Chat ビューのエージェント選択メニューで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを開きます。選択中のエージェントが表示されます（既定では **default**）。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。各編集はファイル パスと差分を含めてチャットに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、エージェントが変更したファイルがわかります。

> [!NOTE]
> この作業には数分かかります。Copilot はリポジトリを実際に変更し、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次のレッスンでは、進行中の作業を確認して追加の指示を送ります。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot がより効果的に作業できるようにします。

次の概念を学びました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は[エージェントの作業を監視して方向を伝えます][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加するよう依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
