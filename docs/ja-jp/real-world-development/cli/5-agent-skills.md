---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、ソフトウェア開発には多くの作業があります。これまで、コードが動作することを手動で確認し、instruction file を使ってコーディング標準に沿うようにしてきました。テスト、lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

こうした作業には、**agent skill** が適しています。Skill を使うと、これらの操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、unit test、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill には、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースが含まれます。エージェントは必要に応じて読み込みます。Skill の基本は、スキル名のフォルダーと、その中の `SKILL.md` という Markdown ファイルです。Markdown には、スキルの名前と説明を定義する frontmatter、機能の概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、スクリプトやほかのリソース用のサブフォルダーも追加できます。

> [!NOTE]
> Skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、既存のプロジェクト コマンドを使うため、`SKILL.md` のみで構成されています。

Skill は、チームで共有して再利用するためにプロジェクトの `.github/skills` フォルダーに置くことも、ユーザーのスキル用フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys のチームが unit test、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明により、Copilot がスキルを呼び出すタイミングを判断しやすくなります。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Skill は、Copilot CLI から直接呼び出したり、自然言語で依頼したりできます。まず、Copilot に3種類のチェックを実行してもらいましょう。

1. フィルター機能の会話に戻り、Interactive モードに切り替えます。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際にどのような結果を返したかが分かるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を反映したスキルを使ってみましょう。Copilot CLI は、会話を再起動しなくても編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. レポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- unit test、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能の PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## スキルの例

次のコミュニティの例は参考資料であり、ワークショップで実施する追加タスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
