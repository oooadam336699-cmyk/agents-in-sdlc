---
title: "レッスン 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、重点的な指示、ベスト プラクティス、コーディング パターン、分野固有の知識を定義すると、特定の作業をより効果的に進めるよう Copilot を導けます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーに置く Markdown ファイル、またはグローバルな `~/.copilot/agents` に置くファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタム エージェントと agent skills の違い

カスタム エージェントと [agent skills][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。最も簡単な区別は、**カスタム エージェントは作業者、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを使って作業を調整します。この演習では、アクセシビリティのカスタム エージェントがサイトをアクセシビリティ ガイドラインに照らして確認し、更新します。その作業の中で、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルなどを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントの作成方法は一つではありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使い、次のことを確認します。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントを使って機能をすばやく実装します。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、アクセシビリティ用のカスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` のフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティ対応の Web サイト用コードを生成する際の主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき間違い。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加の指示を送れます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面。カスタム エージェントと Accessibility が強調表示されています](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot がより効果的に作業できるようにします。

次の概念を学びました。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

Copilot がハイ コントラスト モードを実装している間に、次は Mission Control から[エージェント セッションを監視して方向を伝えます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
