---
title: "レッスン 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前のレッスンでは、ユーザー エクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェント ページ][agents-page]では、オープンなプルリクエスト（PR）があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに新しい機能を依頼します。

## シナリオ

ハイ コントラスト モードを作成するようエージェントに依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイル更新と切り替え機能を作業中なので、これも一緒に実装するのが合理的です。ハイ コントラストに加えてライト モードも追加するよう、エージェントに伝えます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような内容があります。
   - リポジトリをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーを設定する。
   - タスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に追加の指示を送る

現在実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードのタスクに対応するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission Control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。次のようなメッセージが表示されたら作業中です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot のタスクを表示するエージェント ページで、作業中のセッションに指示を送るダイアログが強調表示されています](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に加えたことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取りかかります。メッセージを送信した時点で実行していた手順が終わると、新しい要求を作業に取り込みます。

前回と同じように、完了まで数分かかります。待つ間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して方向を伝える中心的な場所である Copilot agents ページを確認しました。進行中または完了したタスクを追跡し、作業内容を確認し、進行中のタスクの範囲を変更したり指示を追加したりできます。

次の概念を学びました。

- Mission Control とエージェント ページで cloud agent のタスクを監視する。
- 進行中のセッションに追加の機能を依頼する。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成した[プルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
