---
title: "レッスン 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このワークショップでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チームが改善を検討する関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を利用してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人間の監督なしでタスクを実行するため、安全性を維持するためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのは作業に使うブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト（PR）、つまりコードにドキュメントを追加する変更を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は既定で外部リソースへのアクセスが制限されており、外部 MCP サーバーへの接続も制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は完了まで待ちます。数分かかる場合があります。休憩を取るか、これまでに学んだことを振り返りましょう。

4. PR の準備ができたら **Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc comment などのドキュメントが追加されたコードを確認します。実際の変更内容は状況によって異なります。

   TSDoc doc comment、TypeScript の規則、コメント ヘッダーを確認してください。これらはレッスン 2 の冒頭で確認したカスタム指示ファイルによるものです。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す通知があるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業することは、一方通行ではありません。チームのメンバーと同じように、プルリクエストのコメントやコードのインライン コメントで Copilot をタグ付けできます。Copilot はコメントを確認し、それに対応する新しいセッションを開始します。結果は確定的ではないため、必ず通用する具体的な文章を提示することはできません。変更を依頼する例をいくつか紹介します。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をほかのユーザーと同じようにタグ付けし、生成されたドキュメントの変更を依頼するコメントを追加します。上記の例を使うか、コードベースのドキュメントについて別の変更を依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が変更のための新しいセッションを始めることがわかります。
3. **Back to pull request** を選択して、プルリクエストに戻ります。

    ![プルリクエストに戻る画面](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

満足するまで改善を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す通知があるか確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc doc comment を追加したりできます（この Issue を割り当てた時点では、カスタム指示を更新する前でした）。Conversation タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタム エージェントを使って実装したアクセシビリティ機能をレビューします。この PR には、レッスン 3 で依頼したハイ コントラスト モードと、レッスン 4 の Mission Control で依頼したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。完了まで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューします。特に次の点を確認してください。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存するためのローカル ストレージの使用。
   - ハイ コントラスト モードとライト モードに対応する CSS またはスタイルの変更。
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）。
   - モードの切り替えを管理する JavaScript / TypeScript のコード。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す通知があるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えが期待どおりに動作し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティのカスタム エージェントが、アクセシビリティのベスト プラクティスに従って Copilot の実装を導いたことを確認してください。アクセシビリティ上の問題や改善点がある場合は、前の PR と同様にコメントで **@copilot** をタグ付けし、修正を依頼できます。

## 任意の演習 — 依頼を続ける

実際のバックログをエージェントに任せ、自分は別の作業に集中できると、cloud agent の利点を活用できます。習慣づけるために、リポジトリに Issue をいくつか登録して Copilot に割り当ててみましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- このリポジトリのカスタム指示ファイルを確認し、cloud agent の出力に反映されることを後で確認できるようにしました。
- Issue を Copilot cloud agent に割り当て、非同期で環境を準備し、計画して、作業を進める様子を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラスト モードとライト モードの切り替えを追加しました。
- Copilot agents ページを Mission Control として使い、アクセシビリティ セッションを監視して方向を伝えました。
- cloud agent の PR をレビューし、`@copilot` をタグ付けして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、次のハーネスも確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — ターミナルから Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使って、レビュー済みの変更を 2 つ作成します。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作る。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
