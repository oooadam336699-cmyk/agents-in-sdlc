---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、それらを明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールをどのように使うかを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選び、フィルター機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト（PR）を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを作成すると、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown のプロファイルで定義する、専門化された Copilot です。プロファイルにはエージェントの目的、指示、使用できるツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー操作ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がこれらの機能をどう使うかを指示します。これらの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルでは、QA で実施するチェックと、その作業の境界を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

作成したエージェントを使う前に、意図した QA ワークフローと作業範囲がプロファイルに反映されていることを確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. 根拠を示して結果を報告すること、実装コードを変更する前に確認を求めること、変更を commit したりプルリクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足している場合は、続ける前に既定のエージェントへ修正を依頼します。

## Issue を基に QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したあと同じフィルター機能の会話を再開し、QA を選ぶと、Issue と計画時の決定を引き継いで使えます。

1. 次のコマンドで QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。それでも、上記のコマンドで有効にできます。

2. QA エージェントに、次のプロンプトで機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進める様子を確認します。
4. 完了したら、出力されたレポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールをどのように使うかを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルター機能の Issue に照らして結果をレビューしました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[機能の PR をまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
