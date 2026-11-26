---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連のワークフローを体験しました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、フィルター機能の Issue を見つけました。
- 星評価を追加し、ポートフォワードされたブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動でマージしました。
- フィルター機能の Issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol（MCP）サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証（QA）カスタム エージェントを作成して選び、要件、カバレッジ、スキルの結果、ブラウザーでの検証の証拠を確認しました。
- フィルター機能全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認しました。

## リリースしたもの

ワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2つの PR を作成します。

1. **星評価:** ゲームカードに既存の `starRating` と、評価がない場合の状態を明示して表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップの手順を簡略化するため、この作業を1つの PR にまとめています。

## さまざまな方法で検証する

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは unit test、lint、型チェックを実行し、追加した形式で結果を報告しました。QA はこれらの結果に加えて要件とテスト カバレッジをレビューしました。

追加するテストは、実際に不足している範囲を補うものである必要があります。新しいテストが不要という QA の結果が正しい場合もあります。マージを許可する前に、コードと検証の証拠を確認してください。変更があった場合は、関係する証拠も更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールによって、生成される結果が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を考えるとき、**Interactive** は範囲を絞った変更を進めながら確認したいとき、**Autopilot** は範囲が明確なタスクを任せたいときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。*何を*、*なぜ*、*どのように*作るのかを明確に伝えると、結果が大きく変わります。

## さらに確認する

中心的なワークフローを学びました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、長い間作ろうと思っていた小さなアプリなどで活用してください。チームで学んだことを共有し、ほかのメンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も体験する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
